---
layout: post
title: MYSQL数据性能优化 
date: 2015-06-27
category: database
tag: mysql
---

**摘要:**
mysql数据库性能优化小记,介绍了一些MySQL性能优化方法和优化途径用到的工具.例如常见
慢查询等问题.


安装演示数据库：
http://dev.mysql.com/doc/index-other.html

```
D:\sakila-db>cd sakila-db

D:\sakila-db\sakila-db>DIR
 驱动器 D 中的卷没有标签。
 卷的序列号是 DEED-CA78

D:\sakila-db\sakila-db 的目录

2015/04/07  11:14    <DIR>          .
2015/04/07  11:14    <DIR>          ..
2014/11/18  17:05         3,231,472 sakila-data.sql
2014/11/18  17:05            23,145 sakila-schema.sql
2014/11/18  17:05            50,019 sakila.mwb
               3 个文件      3,304,636 字节
               2 个目录 38,402,310,144 可用字节

D:\sakila-db\sakila-db>mysql -uroot -p <sakila-schema.sql
Enter password: *********

D:\sakila-db\sakila-db>mysql -uroot -p <sakila-data.sql
Enter password: *********
ERROR at line 153: Unknown command '\"'.        # 这里出现错误但是可以使用

D:\sakila-db\sakila-db>
```

```
mysql> use sakila;
Database changed
mysql> show tables;
+----------------------------+
| Tables_in_sakila           |
+----------------------------+
| actor                      |
| actor_info                 |
| address                    |
| category                   |
| city                       |
```

```
mysql> SHOW VARIABLES LIKE 'slow_query_log';        # 查看慢查询日志是否打开
+----------------+-------+
| Variable_name  | Value |
+----------------+-------+
| slow_query_log | ON    |
+----------------+-------+
1 row in set (0.00 sec)
```

```
mysql> SHOW VARIABLES LIKE '%log';            # % 是通配符
+--------------------------------+-------+
| Variable_name                  | Value |
+--------------------------------+-------+
| back_log                       | 80    |
| general_log                    | OFF   |
| innodb_api_enable_binlog       | OFF   |
| innodb_locks_unsafe_for_binlog | OFF   |
| relay_log                      |       |
| slow_query_log                 | ON    |
| sync_binlog                    | 0     |
| sync_relay_log                 | 10000 |
+--------------------------------+-------+
8 rows in set (0.00 sec)
```

```
mysql> SET global log_queries_not_using_indexes = ON;
Query OK, 0 rows affected (0.00 sec)
mysql> SET GLOBAL slow_query_log = ON;        # 打开慢查询日志功能
Query OK, 0 rows affected (0.00 sec)

mysql> SHOW variables LIKE 'slow%';        # 可以看到慢查询日志的位置
+---------------------+--------------------+
| Variable_name       | Value              |
+---------------------+--------------------+
| slow_launch_time    | 2                  |
| slow_query_log      | ON                 |
| slow_query_log_file | LINJIE-PC-slow.log |        #  win7 中在 C:\ProgramData\MySQL\MySQL Server 5.6\data 文件夹中
+---------------------+--------------------+
3 rows in set (0.00 sec)

```



分解查看慢查询日志，使用日志分析工具，生成分析报表：


1、mysqldumpslow  MySQL自带的分析工具

```
linjie:~$ mysqldumpslow
Can't find '/var/lib/mysql/*-slow.log'
linjie:~$ 
linjie:~$ 
linjie:~$ mysql -u root -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 39
Server version: 5.5.40-0ubuntu0.14.04.1 (Ubuntu)

Copyright (c) 2000, 2014, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

```

```
mysql> SET GLOBAL log_queries_not_using_indexes=on;
Query OK, 0 rows affected (0.00 sec)

mysql> set global long_query_time=0.01;    # 设置慢查询时间为0.01秒
Query OK, 0 rows affected (0.00 sec)

mysql> SHOW VARIABLES LIKE 'SLOW_QUERY_LOG';
+----------------+-------+
| Variable_name  | Value |
+----------------+-------+
| slow_query_log | OFF   |
+----------------+-------+
1 row in set (0.00 sec)

mysql> SHOW VARIABLES LIKE 'log%';
+---------------------------------+--------------------------+
| Variable_name                   | Value                    |
+---------------------------------+--------------------------+
| log                             | OFF                      |
| log_bin                         | OFF                      |
| log_bin_trust_function_creators | OFF                      |
| log_error                       | /var/log/mysql/error.log |
| log_output                      | FILE                     |
| log_queries_not_using_indexes   | ON                       |
| log_slave_updates               | OFF                      |
| log_slow_queries                | OFF                      |
| log_warnings                    | 1                        |
+---------------------------------+--------------------------+
9 rows in set (0.00 sec)
```

```
mysql> SET GLOBAL slow_query_log=ON;
Query OK, 0 rows affected (0.00 sec)

mysql> USE SAKILA;
ERROR 1049 (42000): Unknown database 'SAKILA'        # 从官网下载sakila 数据库并安装
mysql> 
linjie:sakila-db$ ls
sakila-data.sql  sakila.mwb  sakila-schema.sql
linjie:sakila-db$ 
linjie:sakila-db$ mysql -uroot -p <sakila-schema.sql        # 安装导入数据库
Enter password: 
linjie:sakila-db$ mysql -uroot -p <sakila-data.sql
Enter password: 
linjie:sakila-db$ 
mysql> SHOW VARIABLES LIKE 'slow%';
+---------------------+------------------------------+
| Variable_name       | Value                        |
+---------------------+------------------------------+
| slow_launch_time    | 2                            |
| slow_query_log      | ON                           |
| slow_query_log_file | /var/lib/mysql/Host-slow.log |
+---------------------+------------------------------+
3 rows in set (0.00 sec)

```

root用户下查看慢查日志:

```
root:sakila-db# tail /var/lib/mysql/Host-slow.log
COMMIT;
# User@Host: root[root] @ localhost []
# Query_time: 0.044031  Lock_time: 0.000000 Rows_sent: 0  Rows_examined: 0
SET timestamp=1428388803;
COMMIT;
# Time: 150407 14:43:21
# User@Host: root[root] @ localhost []
# Query_time: 0.001194  Lock_time: 0.000096 Rows_sent: 200  Rows_examined: 200
SET timestamp=1428389001;
select * from actor;
```


使用mysqldumpslow工具来分析慢查日志(内容比较多可以用help来查看使用情况):


```
root:sakila-db# mysqldumpslow -t 3  /var/lib/mysql/Host-slow.log
Count: 1  Time=0.37s (0s)  Lock=0.00s (0s)  Rows=0.0 (0), root[root]@localhost
  CREATE TABLE film_category (
  film_id SMALLINT UNSIGNED NOT NULL,
  category_id TINYINT UNSIGNED NOT NULL,
  last_update TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (film_id, category_id),
  CONSTRAINT fk_film_category_film FOREIGN KEY (film_id) REFERENCES film (film_id) ON DELETE RESTRICT ON UPDATE CASCADE,
  CONSTRAINT fk_film_category_category FOREIGN KEY (category_id) REFERENCES category (category_id) ON DELETE RESTRICT ON UPDATE CASCADE
  )ENGINE=InnoDB DEFAULT CHARSET=utf8

```




在本机中安装apt-get install percona-toolkit才可以使用日志分析工具 pt-query-digest

```
root:linjie# pt-query-digest --help

pt-query-digest analyzes MySQL queries from slow, general, and binary log files.
It can also analyze queries from C<SHOW PROCESSLIST> and MySQL protocol data
from tcpdump.  By default, queries are grouped by fingerprint and reported in
descending order of query time (i.e. the slowest queries first).  If no C<FILES>
are given, the tool reads C<STDIN>.  The optional C<DSN> is used for certain
options like L<"--since"> and L<"--until">.  For more details, please use the
--help option, or try 'perldoc /usr/bin/pt-query-digest' for complete
documentation.

Usage: pt-query-digest [OPTIONS] [FILES] [DSN]



root:linjie# pt-query-digest  /var/lib/mysql/Host-slow.log |more

# 400ms user time, 30ms system time, 42.91M rss, 120.62M vsz
# Current date: Tue Apr  7 15:21:41 2015
# Hostname: Host
# Files: /var/lib/mysql/Host-slow.log
# Overall: 54 total, 38 unique, 0.25 QPS, 0.03x concurrency ______________
# Time range: 2015-04-07 14:39:44 to 14:43:21
# Attribute          total     min     max     avg     95%  stddev  median
# ============     ======= ======= ======= ======= ======= ======= =======
# Exec time             6s     1ms   602ms   116ms   356ms   108ms    87ms
# Lock time          193ms       0    62ms     4ms    16ms    11ms   176us
# Rows sent            200       0     200    3.70       0   25.99       0
# Rows examine         200       0     200    3.70       0   25.99       0
# Query size         2.94M       6 1010.88k  55.72k 201.74k 191.11k  381.65

# Profile
# Rank Query ID           Response time Calls R/Call V/M   Item
# ==== ================== ============= ===== ====== ===== ===============
#    1 0x813031B8BBC3B329  1.2162 19.4%    15 0.0811  0.02 COMMIT
#    2 0xAAFA9D65F37A50E9  0.6257 10.0%     2 0.3128  0.53 INSERT payment
#    3 0xCE1717EC0EDB34A8  0.6051  9.6%     2 0.3026  0.30 INSERT rental
#    4 0xC6A10D5F70E55E2C  0.3678  5.9%     1 0.3678  0.00 CREATE TABLE film_category
#    5 0xFB9930A2CD51E3C4  0.3576  5.7%     1 0.3576  0.00 CREATE TABLE payment
#    6 0x8E4B59B60224A873  0.2123  3.4%     1 0.2123  0.00 CREATE TABLE rental
#    7 0x050C06EA5B5F0A58  0.1909  3.0%     1 0.1909  0.00 INSERT film
#    8 0x0F9C0958963F7FEA  0.1620  2.6%     1 0.1620  0.00 CREATE
#    9 0x704EAFDC6327E6B1  0.1565  2.5%     1 0.1565  0.00 CREATE TABLE staff
#   10 0x7D20F7BC91FA7E09  0.1460  2.3%     1 0.1460  0.00 CREATE
#   11 0x81111B833A29E7E3  0.1454  2.3%     1 0.1454  0.00 CREATE TABLE store
#   12 0x76C2D86A2D27DBE3  0.1344  2.1%     1 0.1344  0.00 CREATE TABLE inventory
#   13 0xDAA404BCF84B6C07  0.1343  2.1%     1 0.1343  0.00 CREATE TABLE film_actor
#   14 0xAA5D67D984519B70  0.1343  2.1%     1 0.1343  0.00 CREATE TABLE film
#   15 0xB81F6955580E244D  0.1343  2.1%     1 0.1343  0.00 CREATE TABLE customer
#   16 0xBF45E32AC4D50022  0.1299  2.1%     1 0.1299  0.00 CREATE TABLE actor
#   17 0x72602773861010BD  0.1025  1.6%     1 0.1025  0.00 CREATE
#   18 0x4C260A99CACE5593  0.1023  1.6%     1 0.1023  0.00 CREATE TABLE address
#   19 0x870A04CA5A67718A  0.1013  1.6%     1 0.1013  0.00 CREATE
#   20 0xC6ED2671EA8DCAE5  0.1007  1.6%     1 0.1007  0.00 CREATE TABLE category
# MISC 0xMISC              1.0186 16.2%    18 0.0566   0.0 <18 ITEMS>

# Query 1: 5 QPS, 0.41x concurrency, ID 0x813031B8BBC3B329 at byte 3088922
# This item is included in the report because it matches --limit.
# Scores: V/M = 0.02
# Time range: 2015-04-07 14:40:00 to 14:40:03
# Attribute    pct   total     min     max     avg     95%  stddev  median
# ============ === ======= ======= ======= ======= ======= ======= =======
# Count         27      15
# Exec time     19      1s    44ms   175ms    81ms   148ms    40ms    63ms
# Lock time      0       0       0       0       0       0       0       0
# Rows sent      0       0       0       0       0       0       0       0
# Rows examine   0       0       0       0       0       0       0       0
# Query size     0      90       6       6       6       6       0       6
# String:
# Databases    sakila
# Hosts        localhost
# Users        root
# Query_time distribution
#   1us
#  10us
# 100us
#   1ms
#  10ms  ################################################################
# 100ms  #######################
#    1s
#  10s+
COMMIT\G

# Query 2: 0 QPS, 0x concurrency, ID 0xAAFA9D65F37A50E9 at byte 630229 ___
# This item is included in the report because it matches --limit.
# Scores: V/M = 0.53
# Time range: all events occurred at 2015-04-07 14:40:01
# Attribute    pct   total     min     max     avg     95%  stddev  median
# ============ === ======= ======= ======= ======= ======= ======= =======
# Count          3       2
--More--

```


count() 和max()的优化方法:	

```
mysql> EXPLAIN SELECT MAX(payment_date) FROM payment\G;	#利用EXPLAIN 关键字列出其后语句执行计划
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE	# 表示是一个简单的select语句
        table: payment	# payment表
         type: ALL	# 全表扫描
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 16371	# 扫描16371行,比较多
        Extra: 
1 row in set (0.00 sec)

ERROR: 
No query specified

```


通常的优化方案是建立索引:

```
mysql> CREATE INDEX idx_paydate ON payment(payment_date);
Query OK, 0 rows affected (0.28 sec)
Records: 0  Duplicates: 0  Warnings: 0

```


再次查看执行计划:

```
mysql> EXPLAIN SELECT MAX(payment_date) FROM payment\G;
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: NULL
         type: NULL
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: NULL
        Extra: Select tables optimized away
1 row in set (0.00 sec)

ERROR: 
No query specified

mysql> 

mysql> CREATE TABLE t (
    -> pid INT
    -> );
Query OK, 0 rows affected (0.15 sec)

mysql> 
mysql> INSERT t VALUES(1);
Query OK, 1 row affected (0.14 sec)

mysql> 
mysql> CREATE TABLE t1 ( pid INT );
Query OK, 0 rows affected (0.10 sec)

mysql> INSERT t1 VALUES(1);
Query OK, 1 row affected (0.08 sec)

```


```
mysql> SELECT * FROM t WHERE t.pid IN (SELECT t1.pid FROM t1);	# 子查询
+------+
| pid  |
+------+
|    1 |
+------+
1 row in set (0.01 sec)

mysql> SELECT t.pid FROM t JOIN t1 ON t.pid = t1.pid;	# 利用链接来优化子查询
+------+
| pid  |
+------+
|    1 |
+------+
1 row in set (0.00 sec)

mysql> INSERT t1 VALUES(1);
Query OK, 1 row affected (0.07 sec)

mysql> SELECT * FROM t WHERE t.pid IN (SELECT t1.pid FROM t1);	# 子查询
+------+
| pid  |
+------+
|    1 |
+------+
1 row in set (0.00 sec)

mysql> SELECT t.pid FROM t JOIN t1 ON t.pid = t1.pid;	# 链接方式优化子查询查询  注意这里如果有一对多的情况 会有多个值
+------+
| pid  |
+------+
|    1 |
|    1 |
+------+
2 rows in set (0.00 sec)

mysql> SELECT DISTINCT t.pid FROM t JOIN t1 ON t.pid = t1.pid;	# 可以利用DISTINCT去掉重复
+------+
| pid  |
+------+
|    1 |
+------+
1 row in set (0.00 sec)

mysql> 

mysql> EXPLAIN SELECT actor.first_name,actor.last_name,COUNT(*) FROM sakila.film_actor INNER JOIN sakila.actor USING(actor_id) GROUP BY film_actor.actor_id \G;
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: actor
         type: ALL	# 使用了表扫描操作
possible_keys: PRIMARY
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 200
        Extra: Using temporary; Using filesort	# 使用了临时表操作
*************************** 2. row ***************************
           id: 1
  select_type: SIMPLE
        table: film_actor
         type: ref
possible_keys: PRIMARY
          key: PRIMARY
      key_len: 2
          ref: sakila.actor.actor_id
         rows: 1
        Extra: Using index
2 rows in set (0.00 sec)

ERROR: 
No query specified

```



为了优化表扫描和使用临时表的操作:

```
mysql> EXPLAIN SELECT actor.first_name,actor.last_name,c.cnt FROM sakila.actor INNER JOIN (SELECT actor_id,COUNT(*) AS cnt FROM sakila.film_actor GROUP BY actor_id) AS c USING(actor_id)\G;
*************************** 1. row ***************************
           id: 1
  select_type: PRIMARY
        table: <derived2>
         type: ALL
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 200
        Extra: 	# 不使用临时表的操作
*************************** 2. row ***************************
           id: 1
  select_type: PRIMARY
        table: actor
         type: eq_ref
possible_keys: PRIMARY
          key: PRIMARY
      key_len: 2
          ref: c.actor_id
         rows: 1
        Extra: 
*************************** 3. row ***************************
           id: 2
  select_type: DERIVED
        table: film_actor
         type: index
possible_keys: NULL
          key: PRIMARY
      key_len: 4
          ref: NULL
         rows: 5585
        Extra: Using index	# 使用索引操作
3 rows in set (0.01 sec)





mysql> EXPLAIN SELECT film_id,description FROM sakila.film ORDER BY title LIMIT 50,5 \G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: film
         type: ALL	# 全表扫描方式
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 1074
        Extra: Using filesort	# 使用了文件方式
1 row in set (0.00 sec)

mysql> 

mysql> EXPLAIN SELECT film_id,description FROM sakila.film ORDER BY film_id LIMIT 50,5 \G # 这里使用了主键
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: film
         type: index	# 索引方式 减少IO
possible_keys: NULL
          key: PRIMARY
      key_len: 2
          ref: NULL
         rows: 55
        Extra: 
1 row in set (0.00 sec)

mysql> 

mysql> EXPLAIN SELECT film_id,description FROM sakila.film WHERE film_id >55 AND film_id <=60  ORDER BY film_id LIMIT 50,5 \G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: film
         type: range	# 范围方式  这里要注意主键要顺序排序的    如果有缺键  有可能会出现不足5行的情况
possible_keys: PRIMARY
          key: PRIMARY
      key_len: 2
          ref: NULL
         rows: 5
        Extra: Using where
1 row in set (0.00 sec)

```
避免过多的扫描.

索引优化注意事项:
选择合适的列建立索引
如何判断离散程度,唯一值越多离散性越好可选择性越多更适合放在联合索引的前面.

```
mysql> SELECT COUNT(DISTINCT customer_id),COUNT(DISTINCT staff_id) FROM payment;
+-----------------------------+--------------------------+
| COUNT(DISTINCT customer_id) | COUNT(DISTINCT staff_id) |
+-----------------------------+--------------------------+
|                         599 |                        2 |
+-----------------------------+--------------------------+
1 row in set (0.01 sec)

mysql> 
这里的customer_id的大.

```



寻找同类型重复的索引并删除掉,来优化索引.

```
mysql> USE information_schema ;	#注意: 下面的查看重复的索引要用到这个数据库的一些表
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> 
mysql> SELECT a.TABLE_SCHEMA AS '数据名',
    -> a.table_name AS '表名',
    -> a.index_name AS '索引1',
    -> b.INDEX_NAME AS '索引2',
    -> a.COLUMN_NAME AS '重复列名'
    -> FROM STATISTICS a JOIN  STATISTICS b ON
    -> a.TABLE_SCHEMA = b.TABLE_SCHEMA AND a.TABLE_NAME=b.TABLE_NAME
    -> AND a.SEQ_IN_INDEX=b.SEQ_IN_INDEX AND a.COLUMN_NAME=
    -> b.COLUMN_NAME WHERE a.SEQ_IN_INDEX=1 AND a.INDEX_NAME<>
    -> b.INDEX_NAME
    -> \G
*************************** 1. row ***************************
   数据名: serverEye
      表名: auth_group_permissions
     索引1: auth_group_permissions_0e939a4f
     索引2: group_id
重复列名: group_id
*************************** 2. row ***************************
   数据名: serverEye
      表名: auth_group_permissions
     索引1: group_id
     索引2: auth_group_permissions_0e939a4f
重复列名: group_id
*************************** 3. row ***************************
   数据名: serverEye
      表名: auth_permission
     索引1: auth_permission_417f1b1c
     索引2: content_type_id
重复列名: content_type_id
*************************** 4. row ***************************
   数据名: serverEye
      表名: auth_permission
     索引1: content_type_id
     索引2: auth_permission_417f1b1c
重复列名: content_type_id
*************************** 5. row ***************************
   数据名: serverEye
      表名: auth_user_groups
     索引1: auth_user_groups_e8701ad4
     索引2: user_id
重复列名: user_id
*************************** 6. row ***************************
   数据名: serverEye
      表名: auth_user_groups
     索引1: user_id
     索引2: auth_user_groups_e8701ad4
重复列名: user_id
*************************** 7. row ***************************
   数据名: serverEye
      表名: auth_user_user_permissions
     索引1: auth_user_user_permissions_e8701ad4
     索引2: user_id
重复列名: user_id
*************************** 8. row ***************************
   数据名: serverEye
      表名: auth_user_user_permissions
     索引1: user_id
     索引2: auth_user_user_permissions_e8701ad4
重复列名: user_id
8 rows in set (0.01 sec)

````

使用工具来处理索引冗余问题:


```
root:linjie# pt-duplicate-key-checker -uroot -p'*********' -h 127.0.0.1

# A software update is available:
#   * The current version for Percona::Toolkit is 2.2.13.

# ########################################################################
# serverEye.auth_group_permissions                                        
# ########################################################################

# auth_group_permissions_0e939a4f is a left-prefix of group_id
# Key definitions:
#   KEY `auth_group_permissions_0e939a4f` (`group_id`),
#   UNIQUE KEY `group_id` (`group_id`,`permission_id`),
# Column types:
#	  `group_id` int(11) not null
#	  `permission_id` int(11) not null
# To remove this duplicate index, execute:
ALTER TABLE `serverEye`.`auth_group_permissions` DROP INDEX `auth_group_permissions_0e939a4f`;

# ########################################################################
# serverEye.auth_permission                                               
# ########################################################################

# auth_permission_417f1b1c is a left-prefix of content_type_id
# Key definitions:
#   KEY `auth_permission_417f1b1c` (`content_type_id`),
#   UNIQUE KEY `content_type_id` (`content_type_id`,`codename`),
# Column types:
#	  `content_type_id` int(11) not null
#	  `codename` varchar(100) not null
# To remove this duplicate index, execute:
ALTER TABLE `serverEye`.`auth_permission` DROP INDEX `auth_permission_417f1b1c`;

# ########################################################################
# serverEye.auth_user_groups                                              
# ########################################################################

# auth_user_groups_e8701ad4 is a left-prefix of user_id
# Key definitions:
#   KEY `auth_user_groups_e8701ad4` (`user_id`),
#   UNIQUE KEY `user_id` (`user_id`,`group_id`),
# Column types:
#	  `user_id` int(11) not null
#	  `group_id` int(11) not null
# To remove this duplicate index, execute:
ALTER TABLE `serverEye`.`auth_user_groups` DROP INDEX `auth_user_groups_e8701ad4`;

# ########################################################################
# serverEye.auth_user_user_permissions                                    
# ########################################################################

# auth_user_user_permissions_e8701ad4 is a left-prefix of user_id
# Key definitions:
#   KEY `auth_user_user_permissions_e8701ad4` (`user_id`),
#   UNIQUE KEY `user_id` (`user_id`,`permission_id`),
# Column types:
#	  `user_id` int(11) not null
#	  `permission_id` int(11) not null
# To remove this duplicate index, execute:
ALTER TABLE `serverEye`.`auth_user_user_permissions` DROP INDEX `auth_user_user_permissions_e8701ad4`;

# ########################################################################
# Summary of indexes                                                      
# ########################################################################

# Size Duplicate Indexes   96
# Total Duplicate Indexes  4
# Total Indexes            130
root:linjie# 
```


使用pt工具处理使用频率低或者不用的索引及键(慎重使用,有的主从服务器猪服务器不用了但是从服务器还是在用的或者相反的情况):

```
pt-index-usage -uroot -p '*****' /var/lib/mysql/Host-slow.log
执行结果:
.......
l Yarn of a Composer And a Man who must Face a Boy in The Canadian Rockies' and 2006 and 1 and NULL and 6 and '0.99' and 105 and '10.99' and 'NC-17' and 'Deleted Scenes' and '2006-02-15 05:03:42') and (999 and 'ZOOLANDER FICTION' and 'A Fateful Reflection of a Waitress And a Boat who must Discover a Sumo Wrestler in Ancient China' and 2006 and 1 and NULL and 5 and '2.99' and 101 and '28.99' and 'R' and 'Trailers and Deleted Scenes' and '2006-02-15 05:03:42') and (1000 and 'ZORRO ARK' and 'A Intrepid Panorama of a Mad Scientist And a Boy who must Redeem a Boy in A Monastery' and 2006 and 1 and NULL and 3 and '4.99' and 50 and '18.99' and 'NC-17' and 'Trailers and Commentaries and Behind the Scenes' and '2006-02-15 05:03:42') "] at /usr/bin/pt-index-usage line 4504, <> line 36.

ALTER TABLE `sakila`.`actor` DROP KEY `idx_actor_last_name`; -- type:non-unique

ALTER TABLE `sakila`.`film` DROP KEY `idx_fk_language_id`, DROP KEY `idx_fk_original_language_id`, DROP KEY `idx_title`; -- type:non-unique

ALTER TABLE `sakila`.`payment` DROP KEY `fk_payment_rental`, DROP KEY `idx_fk_customer_id`, DROP KEY `idx_fk_staff_id`, DROP KEY `idx_paydate`; -- type:non-unique

```


选择合适的数据类型优化数据结构

范式优化,一般情况下遵循第三范式

表的拆分
垂直
水平


系统层级的优化
32位的要更注意
64位的要好一些


```
linjie:~$ ulimit -a        # 查看系统参数
core file size          (blocks, -c) 0
data seg size           (kbytes, -d) unlimited
scheduling priority             (-e) 0
file size               (blocks, -f) unlimited
pending signals                 (-i) 30305
max locked memory       (kbytes, -l) 64
max memory size         (kbytes, -m) unlimited
open files                      (-n) 1024
pipe size            (512 bytes, -p) 8
POSIX message queues     (bytes, -q) 819200
real-time priority              (-r) 0
stack size              (kbytes, -s) 8192
cpu time               (seconds, -t) unlimited
max user processes              (-u) 30305
virtual memory          (kbytes, -v) unlimited
file locks                      (-x) unlimited
linjie:~$ 
```



防火墙软件必须关掉,多使用硬件防火墙,非相关软件都去关掉 

mysql本身配置文件的配置优化

配置文件的顺序可以通过如下查找:

```
linjie:~$ /usr/sbin/mysqld --verbose --help |grep -A 1 'Default options'
150408  8:35:24 [Warning] Using unique option prefix key_buffer instead of key_buffer_size is deprecated and will be removed in a future release. Please use the full name instead. 
150408  8:35:24 [Warning] Can't create test file /var/lib/mysql/Host.lower-test
150408  8:35:24 [Warning] Can't create test file /var/lib/mysql/Host.lower-test
/usr/sbin/mysqld: Can't change dir to '/var/lib/mysql/' (Errcode: 13)
150408  8:35:24 [Warning] One can only use the --user switch if running as root

150408  8:35:24 [Warning] Using unique option prefix myisam-recover instead of myisam-recover-options is deprecated and will be removed in a future release. Please use the full name instead.
150408  8:35:24 [Note] Plugin 'FEDERATED' is disabled.
/usr/sbin/mysqld: Table 'mysql.plugin' doesn't exist
150408  8:35:24 [ERROR] Can't open the mysql.plugin table. Please run mysql_upgrade to create it.
Default options are read from the following files in the given order:
/etc/my.cnf /etc/mysql/my.cnf /usr/etc/my.cnf ~/.my.cnf 
```


配置文件参数:


1. innodb_buffer_pool_instances
5.5 之后的版本新增的参数,可以控制缓冲池的个数用于并发处理提高性能,默认只有一个缓冲池

2. innodb_log_buffer_size
innodb log缓冲大小   日志最长1秒中刷新一次,一般不用太大
3. innodb_flush_log_at_trx_commit 
关键参数,对innodb的IO效率影响很大,默认为1,可以取0,1,2三个值,一般建议设为值2,但如果数据安全性比较高则使用1
4. innodb_read_io_threads
innodb_write_io_threads
IO读写的线程数目 一般设置为CPU核数  或者根据读写的差别设置

5. innodb_file_per_table
关键参数 控制每一个表使用独立的表空间,默认为OFF也就是说有表都会建立在共享表空间中 设置为ON较好
6. innodb_stats_on_metadata
决定了mysql在什么情况下会刷新innodb表的统计信息,一般设置为OFF来提高性能,使用认为的刷新操作.


第三方工具的使用:
http://tools.percona.com/wizard
































