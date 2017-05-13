---
layout: post
title: MySql基础
date: 2015-10-18
tag: mysql 
category: database 
---

**摘要:**
记录mysql数据基础操作内容,增删改查,日期操作,表操作,字段操作.
信息函数,自定义函数,索引操作,数值预算等等数据库操作.存储过程和简单锁策略.

#### MySql Usually Sentence

- show index
`show index from <table-name>`

- update item field value
`update  table_name set field=1 where id=28176`

- select items witch is start with "str"
`Select * from <table_name>  where <field_name> REGEXP "^str";`


---

#### mysql between

`mysql> select * from clicks where id between 90 and 100;`

    ```
    +-----+--------+----------------------+---------------------+---------+---------------------+
    | id  | linkid | openid               | created             | isvalid | click_time          |
    +-----+--------+----------------------+---------------------+---------+---------------------+
    |  90 |      1 | 12341122222222222222 | 2015-08-29 10:47:18 |       0 | 2015-08-29 10:47:18 |
    |  91 |      1 | 12341122222222222222 | 2015-08-29 10:51:36 |       0 | 2015-08-29 10:51:36 |
    |  92 |      1 | 12341122222222222222 | 2015-08-29 10:51:36 |       0 | 2015-08-29 10:51:36 |
    |  93 |      1 | 12341122222222222222 | 2015-08-29 10:51:36 |       0 | 2015-08-29 10:51:36 |
    |  94 |      1 | 12341122222222222222 | 2015-08-29 10:51:36 |       0 | 2015-08-29 10:51:36 |
    |  95 |      1 | 12341122222222222222 | 2015-08-29 10:51:36 |       0 | 2015-08-29 10:51:36 |
    |  96 |      1 | 12341122222222222222 | 2015-08-29 10:51:36 |       0 | 2015-08-29 10:51:36 |
    |  97 |      1 | 12341122222222222222 | 2015-08-29 10:51:36 |       0 | 2015-08-29 10:51:36 |
    |  98 |      1 | 12341122222222222222 | 2015-08-29 10:51:36 |       0 | 2015-08-29 10:51:36 |
    |  99 |      1 | 12341122222222222222 | 2015-08-29 10:51:36 |       0 | 2015-08-29 10:51:36 |
    | 100 |      1 | 12341122222222222222 | 2015-08-29 10:51:36 |       0 | 2015-08-29 10:51:36 |
    +-----+--------+----------------------+---------------------+---------+---------------------+
    11 rows in set (0.00 sec)
    ```

---

#### Assign Prompt

`mysql -u root -p --prompt \h`  # --prompt 表示设置提示符是什么  \h 表示localhost

```
\D  完整日期
\d  当前数据库
\h  当前主机
\u  当前用户
```

`mysql -u root -p`
登陆后输入如下命令: prompt \u@\h \d>


---

#### Base 

```
root@localhost mydb>select user();
+----------------+
| user()         |
+----------------+
| root@localhost |
+----------------+
1 row in set (0.00 sec)

root@localhost mydb>select version();
+------------+
| version()  |
+------------+
| 5.6.21-log |
+------------+
1 row in set (0.00 sec)

root@localhost mydb>
root@localhost mydb>select now();
+---------------------+
| now()               |
+---------------------+
| 2015-04-05 20:52:40 |
+---------------------+
1 row in set (0.00 sec)

root@localhost mydb>


root@localhost mydb>CREATE DATABASE IF NOT EXISTS muke;
Query OK, 1 row affected, 1 warning (0.00 sec)

root@localhost mydb>SHOW WARNINGS;    # 查看警告信息
+-------+------+-----------------------------------------------+
| Level | Code | Message                                       |
+-------+------+-----------------------------------------------+
| Note  | 1007 | Can't create database 'muke'; database exists |
+-------+------+-----------------------------------------------+
1 row in set (0.00 sec)

root@localhost mydb>

root@localhost mydb>SHOW CREATE DATABASE muke; # 查看新建数据库的时候是用的什么命令  查看是什么编码方式
+----------+---------------------------------------------------------------+
| Database | Create Database                                               |
+----------+---------------------------------------------------------------+
| muke     | CREATE DATABASE `muke` /*!40100 DEFAULT CHARACTER SET utf8 */ |
+----------+---------------------------------------------------------------+
1 row in set (0.00 sec)


root@localhost mydb>CREATE DATABASE IF NOT EXISTS t2 CHARACTER SET gbk;  # 指定编码模式
Query OK, 1 row affected (0.00 sec)

root@localhost mydb>SHOW CREATE DATABASE t2;    # 查看编码模式
+----------+------------------------------------------------------------+
| Database | Create Database                                            |
+----------+------------------------------------------------------------+
| t2       | CREATE DATABASE `t2` /*!40100 DEFAULT CHARACTER SET gbk */ |
+----------+------------------------------------------------------------+
1 row in set (0.00 sec)

root@localhost mydb>
root@localhost mydb>ALTER DATABASE t2 CHARACTER SET = UTF8;     # 修改编码方式
Query OK, 1 row affected (0.01 sec)

root@localhost mydb>SHOW CREATE DATABASE t2;    # 查看编码方式
+----------+-------------------------------------------------------------+
| Database | Create Database                                             |
+----------+-------------------------------------------------------------+
| t2       | CREATE DATABASE `t2` /*!40100 DEFAULT CHARACTER SET utf8 */ |
+----------+-------------------------------------------------------------+
1 row in set (0.00 sec)

root@localhost mydb>
root@localhost mydb>DROP DATABASE IF EXISTS t2;# 删除数据库（如果存在的话）
Query OK, 0 rows affected (0.06 sec)
```


---

#### Data Types

- 日期时间类型在实际开发的过程中用的不是很多，多是以时间戳的形式使用


--- 

#### Table 

- mysql -u root -p -P 3306 -h 127.0.0.1    # 登陆连接数据库


```
mysql>USE muke;
Database changed
mysql>
mysql> SELECT DATABASE();    # 查看当前使用的数据库
+------------+
| DATABASE() |
+------------+
| muke       |
+------------+
1 row in set (0.00 sec)
```


1. Create table
    
    ```
    mysql> CREATE TABLE IF NOT EXISTS tb1(
        -> username VARCHAR(20) NOT NULL,
        -> age TINYINT UNSIGNED,
        -> salary FLOAT(8,2) UNSIGNED
        -> );
    Query OK, 0 rows affected (0.34 sec)
    ```

2. Show table
    
    ```
    mysql> SHOW TABLES;    # 查看数据表
    +----------------+
    | Tables_in_muke |
    +----------------+
    | tb1            |
    +----------------+
    1 row in set (0.00 sec)
    ```

    ```
    mysql> SHOW TABLES FROM mysql;    # 从mysql数据中查看表
    +---------------------------+
    | Tables_in_mysql           |
    +---------------------------+
    | columns_priv              |
    | db                        |
    | event                     |
    | func                      |
    | general_log               |
    | help_category             |
    | help_keyword              |
    | help_relation             |
    | help_topic                |
    | innodb_index_stats        |
    | innodb_table_stats        |
    | ndb_binlog_index          |
    | plugin                    |
    | proc                      |
    | procs_priv                |
    | proxies_priv              |
    | servers                   |
    | slave_master_info         |
    | slave_relay_log_info      |
    | slave_worker_info         |
    | slow_log                  |
    | tables_priv               |
    | time_zone                 |
    | time_zone_leap_second     |
    | time_zone_name            |
    | time_zone_transition      |
    | time_zone_transition_type |
    | user                      |
    +---------------------------+
    28 rows in set (0.09 sec)
    ```

3. Show table colums

    ```
    mysql> SHOW COLUMNS FROM tb1;        # 查看数据表的结构(列名称)和数据类型
    +----------+---------------------+------+-----+---------+-------+
    | Field    | Type                | Null | Key | Default | Extra |
    +----------+---------------------+------+-----+---------+-------+
    | username | varchar(20)         | NO  |     | NULL    |       |
    | age      | tinyint(3) unsigned | YES  |     | NULL    |       |
    | salary   | float(8,2) unsigned | YES  |     | NULL    |       |
    +----------+---------------------+------+-----+---------+-------+
    3 rows in set (0.00 sec)
    ```

4. Insert data into table

    ```
    mysql> INSERT tb1 VALUES('TOM',25,7863.65);
    Query OK, 1 row affected (0.10 sec)
    mysql> INSERT tb1 VALUES('MAX',25,7863.65);
    Query OK, 1 row affected (0.06 sec)
    mysql> INSERT tb1(username,salary) VALUES('Linjie',8000);    # 指定列名称插入内容
    Query OK, 1 row affected (0.09 sec)
    mysql> SELECT * FROM tb1;
    +----------+------+---------+
    | username | age  | salary  |
    +----------+------+---------+
    | TOM      |   25 | 7863.65 |
    | MAX      |   25 | 7863.65 |
    | Linjie   | NULL | 8000.00 |
    +----------+------+---------+
    ```

---

#### Key


1. Primary key
    
    ```
    
    mysql> CREATE TABLE tb3 (
        -> id SMALLINT UNSIGNED  AUTO_INCREMENT PRIMARY KEY,
        -> username VARCHAR(20)
        -> );
    Query OK, 0 rows affected (0.62 sec)

    mysql> SHOW COLUMNS FROM tb3;
    +----------+----------------------+------+-----+---------+----------------+
    | Field    | Type                 | Null | Key | Default | Extra          |
    +----------+----------------------+------+-----+---------+----------------+
    | id       | smallint(5) unsigned | NO   | PRI | NULL    | auto_increment |
    | username | varchar(20)          | YES  |     | NULL    |                |
    +----------+----------------------+------+-----+---------+----------------+

    mysql> INSERT tb3 (username) VALUES('tom');
    Query OK, 1 row affected (0.08 sec)
    
    mysql> INSERT tb3 (username) VALUES('tom2');
    Query OK, 1 row affected (0.07 sec)
    
    mysql> INSERT tb3 (username) VALUES('MAX');
    Query OK, 1 row affected (0.06 sec)
    
    mysql> SELECT * FROM tb3;
    +----+----------+
    | id | username |
    +----+----------+
    |  1 | tom      |
    |  2 | tom2     |
    |  3 | MAX      |
    +----+----------+
    ```
    
2. Unique key

    主键在一张表中只能有一个且不能为空且唯一，unique key 一张表可以多个并且可以有空值，保证字段的唯一性.  
    ```
    mysql> CREATE TABLE tb4
        -> (
        -> id SMALLINT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
        -> username VARCHAR(20) NOT NULL UNIQUE KEY,
        -> age TINYINT UNSIGNED);
    Query OK, 0 rows affected (0.50 sec)
    
    mysql> SHOW COLUMNS FROM tb4;
    +----------+----------------------+------+-----+---------+----------------+
    | Field    | Type                 | Null | Key | Default | Extra          |
    +----------+----------------------+------+-----+---------+----------------+
    | id       | smallint(5) unsigned | NO   | PRI | NULL    | auto_increment |
    | username | varchar(20)          | NO   | UNI | NULL    |                |
    | age      | tinyint(3) unsigned  | YES  |     | NULL    |                |
    +----------+----------------------+------+-----+---------+----------------+
    mysql> INSERT tb4(username, age)VALUES('TOM',22) ;
    mysql> INSERT tb4(username, age)VALUES('TOM',22);
    ERROR 1062 (23000): Duplicate entry 'TOM' for key 'username'   # 唯一性约束

    ```

3. Default 

    ```
    mysql> CREATE TABLE tb6(
        -> id SMALLINT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
        -> username VARCHAR(20) NOT NULL UNIQUE KEY,
        -> sex ENUM('M','F','S') DEFAULT 'S'
        -> );
    Query OK, 0 rows affected (0.31 sec)
    
    mysql> SHOW COLUMNS FROM tb6;
    +----------+----------------------+------+-----+---------+----------------+
    | Field    | Type                 | Null | Key | Default | Extra          |
    +----------+----------------------+------+-----+---------+----------------+
    | id       | smallint(5) unsigned | NO   | PRI | NULL    | auto_increment |
    | username | varchar(20)          | NO   | UNI | NULL    |                |
    | sex      | enum('M','F','S')    | YES  |     | S       |                |
    +----------+----------------------+------+-----+---------+----------------+
    3 rows in set (0.02 sec)
    
    mysql>
    mysql> INSERT tb6 (username) VALUES('TOM');
    Query OK, 1 row affected (0.06 sec)
    
    mysql> SELECT * FROM tb6;
    +----+----------+------+
    | id | username | sex  |
    +----+----------+------+
    |  1 | TOM      | S    |
    +----+----------+------+
    1 row in set (0.00 sec)
    ```


4. Forning key

    4.1 父表和子表有相同的数据引擎且为InnoDB，
    
    4.2 外键列和参照列必须具有相似的数据类型，且数字的长度和有无符号必须相同，字符长度可以不同
    
    4.3 外键列和参照列必须创建索引，如果参照列没有索引MySQL会自动创建索引。
    
    ```
    root@127.0.0.1 muke>CREATE TABLE province(
        -> id SMALLINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
        -> pname VARCHAR(20) NOT NULL
        -> );
    Query OK, 0 rows affected (0.55 sec)
    
    root@127.0.0.1 muke>
    root@127.0.0.1 muke>SHOW CREATE TABLE provience;
    ERROR 1146 (42S02): Table 'muke.provience' doesn't exist
    root@127.0.0.1 muke>SHOW CREATE TABLE province;
    
    | Table    | Create Table|
    | province | CREATE TABLE `province` (
      `id` smallint(5) unsigned NOT NULL AUTO_INCREMENT,
      `pname` varchar(20) NOT NULL,
      PRIMARY KEY (`id`)
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8 |

    root@127.0.0.1 muke>CREATE TABLE users(
        -> id SMALLINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
        -> username VARCHAR(10) NOT NULL,
        -> pid SMALLINT UNSIGNED,
        -> FOREIGN KEY (pid) REFERENCES province(id)
        -> );
    Query OK, 0 rows affected (0.29 sec)

    root@127.0.0.1 muke>SHOW CREATE TABLE users;
    
    | Table | Create Table|
    | users | CREATE TABLE `users` (
      `id` smallint(5) unsigned NOT NULL AUTO_INCREMENT,
      `username` varchar(10) NOT NULL,
      `pid` smallint(5) unsigned DEFAULT NULL,
      PRIMARY KEY (`id`),
      KEY `pid` (`pid`),
      CONSTRAINT `users_ibfk_1` FOREIGN KEY (`pid`) REFERENCES `province` (`id`)   # 外键参照列
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8 |

    root@127.0.0.1 muke>SHOW INDEXES FROM province \G;   # \G是表示以网格形式显示   这里的参照咧自动生成索引
    *************************** 1. row ***************************
            Table: province
       Non_unique: 0
         Key_name: PRIMARY
     Seq_in_index: 1
      Column_name: id
        Collation: A
      Cardinality: 0
         Sub_part: NULL
           Packed: NULL
             Null:
       Index_type: BTREE
          Comment:
    Index_comment:
    1 row in set (0.00 sec)
    
    ERROR:
    No query specified
    
    root@127.0.0.1 muke>
    
    root@127.0.0.1 muke>SHOW INDEXES FROM users \G;
    *************************** 1. row ***************************
            Table: users
       Non_unique: 0
         Key_name: PRIMARY
     Seq_in_index: 1
      Column_name: id
        Collation: A
      Cardinality: 0
         Sub_part: NULL
           Packed: NULL
             Null:
       Index_type: BTREE
          Comment:
    Index_comment:
    *************************** 2. row ***************************
            Table: users
       Non_unique: 1
         Key_name: pid
     Seq_in_index: 1
      Column_name: pid
        Collation: A
      Cardinality: 0
         Sub_part: NULL
           Packed: NULL
             Null: YES
       Index_type: BTREE
          Comment:
    Index_comment:
    2 rows in set (0.00 sec)

    ```

5. 外键约束的参照操作
    
    ```
    root@127.0.0.1 muke>CREATE TABLE users1(
        -> id SMALLINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
        -> username VARCHAR(10) NOT NULL,
        -> pid SMALLINT UNSIGNED,
        -> FOREIGN KEY (pid) REFERENCES province(id) ON DELETE CASCADE
        -> );
    Query OK, 0 rows affected (0.31 sec)
    ```

    ```
    root@127.0.0.1 muke>
    root@127.0.0.1 muke>SHOW CREATE TABLE users1;
    | Table  | Create Table|
    | users1 | CREATE TABLE `users1` (
      `id` smallint(5) unsigned NOT NULL AUTO_INCREMENT,
      `username` varchar(10) NOT NULL,
      `pid` smallint(5) unsigned DEFAULT NULL,
      PRIMARY KEY (`id`),
      KEY `pid` (`pid`),
      CONSTRAINT `users1_ibfk_1` FOREIGN KEY (`pid`) REFERENCES `province` (`id`) ON
     DELETE CASCADE
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8 |
    1 row in set (0.00 sec)
    
    root@127.0.0.1 muke>SHOW CREATE TABLE users1 \G;
    *************************** 1. row ***************************
           Table: users1
    Create Table: CREATE TABLE `users1` (
      `id` smallint(5) unsigned NOT NULL AUTO_INCREMENT,
      `username` varchar(10) NOT NULL,
      `pid` smallint(5) unsigned DEFAULT NULL,
      PRIMARY KEY (`id`),
      KEY `pid` (`pid`),
      CONSTRAINT `users1_ibfk_1` FOREIGN KEY (`pid`) REFERENCES `province` (`id`) ON
     DELETE CASCADE
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8
    1 row in set (0.00 sec)

    ```

    ```
    root@127.0.0.1 muke>
    root@127.0.0.1 muke>INSERT province (pname) VALUES('A');
    Query OK, 1 row affected (0.07 sec)
    
    root@127.0.0.1 muke>INSERT province (pname) VALUES('B');
    Query OK, 1 row affected (0.07 sec)
    
    root@127.0.0.1 muke>INSERT province (pname) VALUES('C');
    Query OK, 1 row affected (0.07 sec)
    
    root@127.0.0.1 muke>SELECT * FROM province;
    +----+-------+
    | id | pname |
    +----+-------+
    |  1 | A     |
    |  2 | B     |
    |  3 | C     |
    +----+-------+
    
    root@127.0.0.1 muke>INSERT user1 (username, pid)VALUES('john',7);
    ERROR 1146 (42S02): Table 'muke.user1' doesn't exist
    root@127.0.0.1 muke>INSERT users1 (username, pid)VALUES('john',1);
    Query OK, 1 row affected (0.07 sec)
    
    root@127.0.0.1 muke>INSERT users1 (username, pid)VALUES('john',7);    # 没有对应的参照列 
    ERROR 1452 (23000): Cannot add or update a child row: a foreign key constraint f
    ails (`muke`.`users1`, CONSTRAINT `users1_ibfk_1` FOREIGN KEY (`pid`) REFERENCES
     `province` (`id`) ON DELETE CASCADE)
    root@127.0.0.1 muke>INSERT users1 (username, pid)VALUES('MAX',3);
    Query OK, 1 row affected (0.04 sec)
    
    root@127.0.0.1 muke>SELECT * FROM users1;
    +----+----------+------+
    | id | username | pid  |
    +----+----------+------+
    |  1 | john     |    1 |
    |  3 | MAX      |    3 |
    +----+----------+------+
    2 rows in set (0.00 sec)        # 这里没有id为2的是因为，>INSERT users1 (username, pid)VALUES('john',7);的时候自增长了1
    
    root@127.0.0.1 muke>
    root@127.0.0.1 muke>DELETE FROM province WHERE id=3;
    Query OK, 1 row affected (0.06 sec)
    
    root@127.0.0.1 muke>SELECT * FROM user1;        #ON DELETE CASCADE 的作用子表中删除情况的更新
    ERROR 1146 (42S02): Table 'muke.user1' doesn't exist
    root@127.0.0.1 muke>SELECT * FROM users1;
    +----+----------+------+
    | id | username | pid  |
    +----+----------+------+
    |  1 | john     |    1 |
    +----+----------+------+
    ```

    **注意**实际的项目开发中，不去定义物理的外键，多去定义逻辑外键，很少用FOREIGN KEY来定义
    
    **注意**:实际开发过程中很少用 表级约束多用列级约束。
    
    ```
    root@127.0.0.1 muke>SHOW COLUMNS FROM users1;
    +----------+----------------------+------+-----+---------+----------------+
    | Field    | Type                 | Null | Key | Default | Extra          |
    +----------+----------------------+------+-----+---------+----------------+
    | id       | smallint(5) unsigned | NO   | PRI | NULL    | auto_increment |
    | username | varchar(10)          | NO   |     | NULL    |                |
    | pid      | smallint(5) unsigned | YES  | MUL | NULL    |                |
    +----------+----------------------+------+-----+---------+----------------+

    ```

6. Table modity

    ```
        root@127.0.0.1 muke>ALTER TABLE users1 ADD age TINYINT UNSIGNED NOT NULL DEFAULT 10;
        Query OK, 0 rows affected (1.08 sec)
        Records: 0  Duplicates: 0  Warnings: 0
        
        root@127.0.0.1 muke>ALTER TABLE users1 ADD passwd VARCHARA(20) NOT NULL AFTER username;
        ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that
        corresponds to your MySQL server version for the right syntax to use near 'VARCH
        ARA(20) NOT NULL AFTER username' at line 1
        root@127.0.0.1 muke>ALTER TABLE users1 ADD passwd VARCHAR(20) NOT NULL AFTER username;
        Query OK, 0 rows affected (1.09 sec)
        Records: 0  Duplicates: 0  Warnings: 0
        
    ```

    **注意**:添加多列的时候只能添加到表的最后面。


    - 删除列：
    
    ```
    root@127.0.0.1 muke>ALTER TABLE users1 DROP passwd;
    Query OK, 0 rows affected (1.07 sec)
    Records: 0  Duplicates: 0  Warnings: 0
    
    root@127.0.0.1 muke>SHOW COLUMnS FROM users1;
    +----------+----------------------+------+-----+---------+----------------+
    | Field    | Type                 | Null | Key | Default | Extra          |
    +----------+----------------------+------+-----+---------+----------------+
    | id       | smallint(5) unsigned | NO   | PRI | NULL    | auto_increment |
    | username | varchar(10)          | NO   |     | NULL    |                |
    | pid      | smallint(5) unsigned | YES  | MUL | NULL    |                |
    | age      | tinyint(3) unsigned  | NO   |     | 10      |                |
    +----------+----------------------+------+-----+---------+----------------+
    4 rows in set (0.00 sec)
    ```


    - 添加主键约束：

    ```
    root@127.0.0.1 muke>CREATE TABLE users2(
    -> username VARCHAR(20) NOT NULL,
    -> pid SMALLINT UNSIGNED
    -> );
    Query OK, 0 rows affected (0.71 sec)
    ```

    ```
    root@127.0.0.1 muke>ALTER TABLE users2 ADD id SMALLINT UNSIGNED;
    Query OK, 0 rows affected (0.69 sec)
    Records: 0  Duplicates: 0  Warnings: 0
    
    root@127.0.0.1 muke>ALTER TABLE users2 ADD CONSTRAINT PK_user2_id PRIMARY KEY (id);    #  CONSTRAINT PK_user2_id 表示添加操作的说明
    Query OK, 0 rows affected (0.76 sec)
    Records: 0  Duplicates: 0  Warnings: 0
    ```

    - 添加唯一约束：

    ```
    root@127.0.0.1 muke>ALTER TABLE users2 ADD UNIQUE(username);
    Query OK, 0 rows affected (0.32 sec)
    Records: 0  Duplicates: 0  Warnings: 0
    
    root@127.0.0.1 muke>SHOW CREATE TABLE users2;
    | users2 | CREATE TABLE `users2` (
      `username` varchar(20) NOT NULL,
      `pid` smallint(5) unsigned DEFAULT NULL,
      `id` smallint(5) unsigned NOT NULL DEFAULT '0',
      PRIMARY KEY (`id`),
      UNIQUE KEY `username` (`username`)
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8 |
    ```

    - 添加外键约束：

    ```
    root@127.0.0.1 muke>SHOW COLUMNS from  province;
    +-------+----------------------+------+-----+---------+----------------+
    | Field | Type                 | Null | Key | Default | Extra          |
    +-------+----------------------+------+-----+---------+----------------+
    | id    | smallint(5) unsigned | NO   | PRI | NULL    | auto_increment |
    | pname | varchar(20)          | NO   |     | NULL    |                |
    +-------+----------------------+------+-----+---------+----------------+
    
    root@127.0.0.1 muke>ALTER TABLE users2 ADD FOREIGN KEY (pid) REFERENCES province (id);
    root@127.0.0.1 muke>SHOW CREATE TABLE users2;
    | users2 | CREATE TABLE `users2` (
      `username` varchar(20) NOT NULL,
      `pid` smallint(5) unsigned DEFAULT NULL,
      `id` smallint(5) unsigned NOT NULL DEFAULT '0',
      PRIMARY KEY (`id`),
      UNIQUE KEY `username` (`username`),
      KEY `pid` (`pid`),
      CONSTRAINT `users2_ibfk_1` FOREIGN KEY (`pid`) REFERENCES `province` (`id`)
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8 |
    ```

    - 添加默认约束：

    ```
    root@127.0.0.1 muke>ALTER TABLE users2 ADD age TINYINT UNSIGNED NOT NULL;
    root@127.0.0.1 muke>SHOW COLUMNS FROM users2;
    +----------+----------------------+------+-----+---------+-------+
    | Field    | Type                 | Null | Key | Default | Extra |
    +----------+----------------------+------+-----+---------+-------+
    | username | varchar(20)          | NO   | UNI | NULL    |       |
    | pid      | smallint(5) unsigned | YES  | MUL | NULL    |       |
    | id       | smallint(5) unsigned | NO   | PRI | 0       |       |
    | age      | tinyint(3) unsigned  | NO   |     | NULL    |       |
    +----------+----------------------+------+-----+---------+-------+
    
    root@127.0.0.1 muke>ALTER TABLE users2 ALTER age SET DEFAULT 15;
    root@127.0.0.1 muke>SHOW COLUMNS FROM users2;
    +----------+----------------------+------+-----+---------+-------+
    | Field    | Type                 | Null | Key | Default | Extra |
    +----------+----------------------+------+-----+---------+-------+
    | username | varchar(20)          | NO   | UNI | NULL    |       |
    | pid      | smallint(5) unsigned | YES  | MUL | NULL    |       |
    | id       | smallint(5) unsigned | NO   | PRI | 0       |       |
    | age      | tinyint(3) unsigned  | NO   |     | 15      |       |
    +----------+----------------------+------+-----+---------+-------+
    ```

    ```
    root@127.0.0.1 muke>ALTER TABLE users2 ALTER age DROP DEFAULT;
    root@127.0.0.1 muke>SHOW COLUMNS FROM users2;
    +----------+----------------------+------+-----+---------+-------+
    | Field    | Type                 | Null | Key | Default | Extra |
    +----------+----------------------+------+-----+---------+-------+
    | username | varchar(20)          | NO   | UNI | NULL    |       |
    | pid      | smallint(5) unsigned | YES  | MUL | NULL    |       |
    | id       | smallint(5) unsigned | NO   | PRI | 0       |       |
    | age      | tinyint(3) unsigned  | NO   |     | NULL    |       |
    +----------+----------------------+------+-----+---------+-------+
    
    root@127.0.0.1 muke>SHOW COLUMNS FROM users2;
    +----------+----------------------+------+-----+---------+-------+
    | Field    | Type                 | Null | Key | Default | Extra |
    +----------+----------------------+------+-----+---------+-------+
    | username | varchar(20)          | NO   | UNI | NULL    |       |
    | pid      | smallint(5) unsigned | YES  | MUL | NULL    |       |
    | id       | smallint(5) unsigned | NO   | PRI | 0       |       |
    | age      | tinyint(3) unsigned  | NO   |     | NULL    |       |
    +----------+----------------------+------+-----+---------+-------+
    ```

    ```
    root@127.0.0.1 muke>ALTER TABLE users2 DROP  PRIMARY KEY;
    root@127.0.0.1 muke>SHOW COLUMNS FROM users2;
    +----------+----------------------+------+-----+---------+-------+
    | Field    | Type                 | Null | Key | Default | Extra |
    +----------+----------------------+------+-----+---------+-------+
    | username | varchar(20)          | NO   | PRI | NULL    |       |
    | pid      | smallint(5) unsigned | YES  | MUL | NULL    |       |
    | id       | smallint(5) unsigned | NO   |     | 0       |       |
    | age      | tinyint(3) unsigned  | NO   |     | NULL    |       |
    +----------+----------------------+------+-----+---------+-------+
    
    root@127.0.0.1 muke>SHOW INDEXES FROM users2 \G;
    *************************** 1. row ***************************
            Table: users2
       Non_unique: 0
         Key_name: username          # 约束名称
     Seq_in_index: 1
      Column_name: username
        Collation: A
      Cardinality: 0
         Sub_part: NULL
           Packed: NULL
             Null:
       Index_type: BTREE
          Comment:
    Index_comment:
    *************************** 2. row ***************************
            Table: users2
       Non_unique: 1
         Key_name: pid
     Seq_in_index: 1
      Column_name: pid
        Collation: A
      Cardinality: 0
         Sub_part: NULL
           Packed: NULL
             Null: YES
       Index_type: BTREE
          Comment:
    Index_comment:
    2 rows in set (0.00 sec)
    ```

    ```
    ERROR:
    No query specified
    
    root@127.0.0.1 muke>ALTER TABLE users2 DROP INDEX username;        #  删除索引
    Query OK, 0 rows affected (0.89 sec)
    Records: 0  Duplicates: 0  Warnings: 0
    root@127.0.0.1 muke>SHOW COLUMNS FROM users2;
    +----------+----------------------+------+-----+---------+-------+
    | Field    | Type                 | Null | Key | Default | Extra |
    +----------+----------------------+------+-----+---------+-------+
    | username | varchar(20)          | NO   |     | NULL    |       |
    | pid      | smallint(5) unsigned | YES  | MUL | NULL    |       |
    | id       | smallint(5) unsigned | NO   |     | 0       |       |
    | age      | tinyint(3) unsigned  | NO   |     | NULL    |       |
    +----------+----------------------+------+-----+---------+-------+
    4 rows in set (0.00 sec)
    
    root@127.0.0.1 muke>SHOW INDExES FROM users2 \G;        # 查看索引
    *************************** 1. row ***************************
            Table: users2
       Non_unique: 1
         Key_name: pid
     Seq_in_index: 1
      Column_name: pid
        Collation: A
      Cardinality: 0
         Sub_part: NULL
           Packed: NULL
             Null: YES
       Index_type: BTREE
          Comment:
    Index_comment:
    1 row in set (0.00 sec)
    ERROR:
    No query specified
    ```

    - 删除外键约束：

   ```
   root@127.0.0.1 muke>SHOW CREATE TABLE users2;
   | users2 | CREATE TABLE `users2` (
     `username` varchar(20) NOT NULL,
     `pid` smallint(5) unsigned DEFAULT NULL,
     `id` smallint(5) unsigned NOT NULL DEFAULT '0',
     `age` tinyint(3) unsigned NOT NULL,
     KEY `pid` (`pid`),
     CONSTRAINT `users2_ibfk_1` FOREIGN KEY (`pid`) REFERENCES `province` (`id`)    # 外键名称
   ) ENGINE=InnoDB DEFAULT CHARSET=utf8 |
   
   root@127.0.0.1 muke>ALTER TABLE users2 DROP FOREIGN KEY users2_ibfk_1;   # 删除外键
   Query OK, 0 rows affected (0.13 sec)
   Records: 0  Duplicates: 0  Warnings: 0
   
   root@127.0.0.1 muke>SHOW CREATE TABLE users2;
   | users2 | CREATE TABLE `users2` (
     `username` varchar(20) NOT NULL,
     `pid` smallint(5) unsigned DEFAULT NULL,
     `id` smallint(5) unsigned NOT NULL DEFAULT '0',
     `age` tinyint(3) unsigned NOT NULL,
     KEY `pid` (`pid`)
   ) ENGINE=InnoDB DEFAULT CHARSET=utf8 |
   ```

   - 数据类型有问题，位置存在问题，

   ```
   root@127.0.0.1 muke>ALTER TABLE users2 MODIFY id SMALLINT UNSIGNED NOT NULL FIRST;
   Query OK, 0 rows affected (0.45 sec)
   Records: 0  Duplicates: 0  Warnings: 0
   
   root@127.0.0.1 muke>SHOW COLUMNS FROM users2;
   +----------+----------------------+------+-----+---------+-------+
   | Field    | Type                 | Null | Key | Default | Extra |
   +----------+----------------------+------+-----+---------+-------+
   | id       | smallint(5) unsigned | NO   |     | NULL    |       |
   | username | varchar(20)          | NO   |     | NULL    |       |
   | pid      | smallint(5) unsigned | YES  | MUL | NULL    |       |
   | age      | tinyint(3) unsigned  | NO   |     | NULL    |       |
   +----------+----------------------+------+-----+---------+-------+
   4 rows in set (0.00 sec)
   
   root@127.0.0.1 muke>ALTER TABLE users2 MODIFY id TINYINT UNSIGNED NOT NULL FIRST        #  注意这里如果从小的数据类型修改到大的数据类型 有可能会造成数据丢失
   ;
   Query OK, 0 rows affected (0.97 sec)
   Records: 0  Duplicates: 0  Warnings: 0
   
   root@127.0.0.1 muke>SHOW COLUMNS FROM users2;
   +----------+----------------------+------+-----+---------+-------+
   | Field    | Type                 | Null | Key | Default | Extra |
   +----------+----------------------+------+-----+---------+-------+
   | id       | tinyint(3) unsigned  | NO   |     | NULL    |       |
   | username | varchar(20)          | NO   |     | NULL    |       |
   | pid      | smallint(5) unsigned | YES  | MUL | NULL    |       |
   | age      | tinyint(3) unsigned  | NO   |     | NULL    |       |
   +----------+----------------------+------+-----+---------+-------+
   4 rows in set (0.00 sec)
   ```


   ```
   root@127.0.0.1 muke>ALTER TABLE users2 CHANGE pid p_id TINYINT UNSIGNED NOT NULL;        # CHANGE 的功能更多（可以修改列名称）
   Query OK, 0 rows affected (0.87 sec)
   Records: 0  Duplicates: 0  Warnings: 0
   
   root@127.0.0.1 muke>SHOW COLUMNS FROM users2;
   +----------+---------------------+------+-----+---------+-------+
   | Field    | Type                | Null | Key | Default | Extra |
   +----------+---------------------+------+-----+---------+-------+
   | id       | tinyint(3) unsigned | NO   |     | NULL    |       |
   | username | varchar(20)         | NO   |     | NULL    |       |
   | p_id     | tinyint(3) unsigned | NO   | MUL | NULL    |       |
   | age      | tinyint(3) unsigned | NO   |     | NULL    |       |
   +----------+---------------------+------+-----+---------+-------+
   4 rows in set (0.01 sec)
   ```

   - 修改数据表的名称：

     **注意：尽量少去修改数据表名称  ，因为有索引已经存储过程的影响。**

   ```
   root@127.0.0.1 muke>ALTER TABLE users2 RENAME users3;
   Query OK, 0 rows affected (0.19 sec)
   
   root@127.0.0.1 muke>show tables;
   +----------------+
   | Tables_in_muke |
   +----------------+
   | province       |
   | tb1            |
   | tb3            |
   | tb4            |
   | tb6            |
   | users          |
   | users1         |
   | users3         |
   +----------------+
   8 rows in set (0.00 sec)
   ```
   
   ```
   root@127.0.0.1 muke>RENAME  TABLE users3 TO users2;        # 可以多张表修改
   Query OK, 0 rows affected (0.20 sec)
   
   root@127.0.0.1 muke>show tables;
   +----------------+
   | Tables_in_muke |
   +----------------+
   | province       |
   | tb1            |
   | tb3            |
   | tb4            |
   | tb6            |
   | users          |
   | users1         |
   | users2         |
   +----------------+
   8 rows in set (0.00 sec)
   ```

---


#### INSERT 语句

   ```
   root@127.0.0.1 muke>CREATE TABLE users4(
       -> id SMALLINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
       -> username VARCHAR(20) NOT NULL,
       -> passwd VARCHAR(20) NOT NULL,
       -> age TINYINT UNSIGNED NOT NULL DEFAULT 18,
       -> sex BOOLEAN
       -> );
   Query OK, 0 rows affected (0.41 sec)
   
   ...
   
   mysql> SELECT * FROM users4;
   +----+----------+--------+-----+------+
   | id | username | passwd | age | sex  |
   +----+----------+--------+-----+------+
   |  1 | JOHN     | 123    |  24 |    1 |
   |  2 | JOHN     | 123    |  24 |    1 |
   |  3 | MAX      | 223    |  24 |    1 |
   |  4 | JOHN     | 123    |  22 |    1 |
   |  5 | JOHN     | 123    |  18 |    1 |
   +----+----------+--------+-----+------+
   5 rows in set (0.00 sec)
   
   mysql>
   mysql> INSERT users4 VALUES(NULL,'lINJIE','520',25,1),(NULL,'ZHANG',md5'112',24,1);
   ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that
   corresponds to your MySQL server version for the right syntax to use near ''112'
   ,24,1)' at line 1
   mysql> INSERT users4 VALUES(NULL,'lINJIE','520',25,1),(NULL,'ZHANG',md5('112'),2
   4,1);
   ERROR 1406 (22001): Data too long for column 'passwd' at row 2    # 提示不够长，无法存储
   mysql>
   mysql> ALTER TABLE users4 MODIFY passwd VARCHAR(32) NOT NULL;
   Query OK, 5 rows affected (0.87 sec)
   Records: 5  Duplicates: 0  Warnings: 0
   
   mysql> INSERT users4 VALUES(NULL,'lINJIE','520',25,1),(NULL,'ZHANG',md5('112'),24,1);
   Query OK, 2 rows affected (0.08 sec)
   Records: 2  Duplicates: 0  Warnings: 0
   
   mysql> SELECT * FROM users4;
   +----+----------+----------------------------------+-----+------+
   | id | username | passwd                           | age | sex  |
   +----+----------+----------------------------------+-----+------+
   |  1 | JOHN     | 123                              |  24 |    1 |
   |  2 | JOHN     | 123                              |  24 |    1 |
   |  3 | MAX      | 223                              |  24 |    1 |
   |  4 | JOHN     | 123                              |  22 |    1 |
   |  5 | JOHN     | 123                              |  18 |    1 |
   |  8 | lINJIE   | 520                              |  25 |    1 |
   |  9 | ZHANG    | 7f6ffaa6bb0b408017b62254211691b5 |  24 |    1 |
   +----+----------+----------------------------------+-----+------+
   7 rows in set (0.00 sec)
   ```
   
   ```
   mysql> SHOW CREATE TABLE users4;
   | users4 | CREATE TABLE `users4` (
     `id` smallint(5) unsigned NOT NULL AUTO_INCREMENT,
     `username` varchar(20) NOT NULL,
     `passwd` varchar(32) NOT NULL,
     `age` tinyint(3) unsigned NOT NULL DEFAULT '18',
     `sex` tinyint(1) DEFAULT NULL,
     PRIMARY KEY (`id`)
   ) ENGINE=InnoDB AUTO_INCREMENT=10 DEFAULT CHARSET=utf8 |
   ```

   - 可以使用子查询的插入语句 INSERT SET(只能插入一条记录)：

    ```
    mysql> INSERT users4 SET username='wutian',passwd=md5('abc');
    mysql> SELECT * FROM users4;
    +----+----------+----------------------------------+-----+------+
    | id | username | passwd                           | age | sex  |
    +----+----------+----------------------------------+-----+------+
    |  1 | JOHN     | 123                              |  24 |    1 |
    |  2 | JOHN     | 123                              |  24 |    1 |
    |  3 | MAX      | 223                              |  24 |    1 |
    |  4 | JOHN     | 123                              |  22 |    1 |
    |  5 | JOHN     | 123                              |  18 |    1 |
    |  8 | lINJIE   | 520                              |  25 |    1 |
    |  9 | ZHANG    | 7f6ffaa6bb0b408017b62254211691b5 |  24 |    1 |
    | 10 | wutian   | 900150983cd24fb0d6963f7d28e17f72 |  18 | NULL |
    +----+----------+----------------------------------+-----+------+
    ```

    - 单表更新：

    ```
    mysql> UPDATE users4 SET age = age -5,sex = 0;
    mysql> SELECT * FROM users4;
    +----+----------+----------------------------------+-----+------+
    | id | username | passwd                           | age | sex  |
    +----+----------+----------------------------------+-----+------+
    |  1 | JOHN     | 123                              |  24 |    0 |
    |  2 | JOHN     | 123                              |  24 |    0 |
    |  3 | MAX      | 223                              |  24 |    0 |
    |  4 | JOHN     | 123                              |  22 |    0 |
    |  5 | JOHN     | 123                              |  18 |    0 |
    |  8 | lINJIE   | 520                              |  25 |    0 |
    |  9 | ZHANG    | 7f6ffaa6bb0b408017b62254211691b5 |  24 |    0 |
    | 10 | wutian   | 900150983cd24fb0d6963f7d28e17f72 |  18 |    0 |
    +----+----------+----------------------------------+-----+------+
    8 rows in set (0.00 sec)
    
    mysql> UPDATE users4 SET age = age +3 WHERE id %2 = 0;
    Query OK, 4 rows affected (0.06 sec)
    Rows matched: 4  Changed: 4  Warnings: 0
    
    mysql> SELECT * FROM users4;
    +----+----------+----------------------------------+-----+------+
    | id | username | passwd                           | age | sex  |
    +----+----------+----------------------------------+-----+------+
    |  1 | JOHN     | 123                              |  24 |    0 |
    |  2 | JOHN     | 123                              |  27 |    0 |
    |  3 | MAX      | 223                              |  24 |    0 |
    |  4 | JOHN     | 123                              |  25 |    0 |
    |  5 | JOHN     | 123                              |  18 |    0 |
    |  8 | lINJIE   | 520                              |  28 |    0 |
    |  9 | ZHANG    | 7f6ffaa6bb0b408017b62254211691b5 |  24 |    0 |
    | 10 | wutian   | 900150983cd24fb0d6963f7d28e17f72 |  21 |    0 |
    +----+----------+----------------------------------+-----+------+
    8 rows in set (0.00 sec)
    ```

---

#### 删除记录DELETE:

    - 单表删除

    ```
    mysql>DELETE FROM users4 WHERE id = 6;
    Query OK, 0 rows affected (0.00 sec)
    
    mysql> SELECT * FROM users4;
    +----+----------+----------------------------------+-----+------+
    | id | username | passwd                           | age | sex  |
    +----+----------+----------------------------------+-----+------+
    |  1 | JOHN     | 123                              |  24 |    0 |
    |  2 | JOHN     | 123                              |  27 |    0 |
    |  3 | MAX      | 223                              |  24 |    0 |
    |  4 | JOHN     | 123                              |  25 |    0 |
    |  5 | JOHN     | 123                              |  18 |    0 |
    |  8 | lINJIE   | 520                              |  28 |    0 |
    |  9 | ZHANG    | 7f6ffaa6bb0b408017b62254211691b5 |  24 |    0 |
    | 10 | wutian   | 900150983cd24fb0d6963f7d28e17f72 |  21 |    0 |
    +----+----------+----------------------------------+-----+------+
    8 rows in set (0.00 sec)
    mysql> INSERT users4 SET username='wutian',passwd=md5('abc');
    Query OK, 1 row affected (0.05 sec)
    
    mysql> SELECT * FROM users4;
    +----+----------+----------------------------------+-----+------+
    | id | username | passwd                           | age | sex  |
    +----+----------+----------------------------------+-----+------+
    |  1 | JOHN     | 123                              |  24 |    0 |
    |  2 | JOHN     | 123                              |  27 |    0 |
    |  3 | MAX      | 223                              |  24 |    0 |
    |  4 | JOHN     | 123                              |  25 |    0 |
    |  5 | JOHN     | 123                              |  18 |    0 |
    |  8 | lINJIE   | 520                              |  28 |    0 |
    |  9 | ZHANG    | 7f6ffaa6bb0b408017b62254211691b5 |  24 |    0 |
    | 10 | wutian   | 900150983cd24fb0d6963f7d28e17f72 |  21 |    0 |
    | 11 | wutian   | 900150983cd24fb0d6963f7d28e17f72 |  18 | NULL |
    +----+----------+----------------------------------+-----+------+
    
    ```

---

#### SELECT


    - 查询函数

    ```
    mysql> SELECT VERSION();
    +------------+
    | VERSION()  |
    +------------+
    | 5.6.21-log |
    +------------+
    1 row in set (0.00 sec)
    
    mysql> SELECT NOW();
    +---------------------+
    | NOW()               |
    +---------------------+
    | 2015-04-06 14:32:49 |
    +---------------------+
    1 row in set (0.00 sec)
    ```

    ```    
    mysql> SELECT sex FROM users4 GROUP BY sex;
    +------+
    | sex  |
    +------+
    | NULL |
    |    0 |
    +------+
    2 rows in set (0.00 sec)
    ```

    - 分组条件指定：

    ```
    mysql> SELECT id,age FROM users4 GROUP BY id HAVING age > 22;
    +----+-----+
    | id | age |
    +----+-----+
    |  1 |  24 |
    |  2 |  27 |
    |  3 |  24 |
    |  4 |  25 |
    |  8 |  28 |
    |  9 |  24 |
    +----+-----+
    6 rows in set (0.00 sec)
    
    mysql> SELECT sex FROM users4 GROUP BY id HAVING count(id) >= 3;
    Empty set (0.00 sec)
    
    mysql> SELECT sex FROM users4 GROUP BY id HAVING count(id) >= 2;
    Empty set (0.00 sec)
    
    mysql> SELECT age FROM users4 GROUP BY id HAVING count(id) >= 2;
    Empty set (0.00 sec)
    ```


    - order by排序：

    ```
    mysql> SELECT * FROM users4 ORDER BY id DESC ;
    +----+----------+----------------------------------+-----+------+
    | id | username | passwd                           | age | sex  |
    +----+----------+----------------------------------+-----+------+
    | 11 | wutian   | 900150983cd24fb0d6963f7d28e17f72 |  18 | NULL |
    | 10 | wutian   | 900150983cd24fb0d6963f7d28e17f72 |  21 |    0 |
    |  9 | ZHANG    | 7f6ffaa6bb0b408017b62254211691b5 |  24 |    0 |
    |  8 | lINJIE   | 520                              |  28 |    0 |
    |  5 | JOHN     | 123                              |  18 |    0 |
    |  4 | JOHN     | 123                              |  25 |    0 |
    |  3 | MAX      | 223                              |  24 |    0 |
    |  2 | JOHN     | 123                              |  27 |    0 |
    |  1 | JOHN     | 123                              |  24 |    0 |
    +----+----------+----------------------------------+-----+------+
    9 rows in set (0.00 sec)
    
    mysql> SELECT * FROM users4 ORDER BY age,id DESC;    # 年龄相同则按照id降序排列
    +----+----------+----------------------------------+-----+------+
    | id | username | passwd                           | age | sex  |
    +----+----------+----------------------------------+-----+------+
    | 11 | wutian   | 900150983cd24fb0d6963f7d28e17f72 |  18 | NULL |
    |  5 | JOHN     | 123                              |  18 |    0 |
    | 10 | wutian   | 900150983cd24fb0d6963f7d28e17f72 |  21 |    0 |
    |  9 | ZHANG    | 7f6ffaa6bb0b408017b62254211691b5 |  24 |    0 |
    |  3 | MAX      | 223                              |  24 |    0 |
    |  1 | JOHN     | 123                              |  24 |    0 |
    |  4 | JOHN     | 123                              |  25 |    0 |
    |  2 | JOHN     | 123                              |  27 |    0 |
    |  8 | lINJIE   | 520                              |  28 |    0 |
    +----+----------+----------------------------------+-----+------+
    9 rows in set (0.01 sec)
    ```


    - limit 限制

    ```
    mysql> SELECT * FROM users4 ORDER BY age,id DESC LIMIT 2;
    +----+----------+----------------------------------+-----+------+
    | id | username | passwd                           | age | sex  |
    +----+----------+----------------------------------+-----+------+
    | 11 | wutian   | 900150983cd24fb0d6963f7d28e17f72 |  18 | NULL |
    |  5 | JOHN     | 123                              |  18 |    0 |
    +----+----------+----------------------------------+-----+------+
    2 rows in set (0.00 sec)
    
    mysql> SELECT * FROM users4 ORDER BY age,id DESC LIMIT 4;
    +----+----------+----------------------------------+-----+------+
    | id | username | passwd                           | age | sex  |
    +----+----------+----------------------------------+-----+------+
    | 11 | wutian   | 900150983cd24fb0d6963f7d28e17f72 |  18 | NULL |
    |  5 | JOHN     | 123                              |  18 |    0 |
    | 10 | wutian   | 900150983cd24fb0d6963f7d28e17f72 |  21 |    0 |
    |  9 | ZHANG    | 7f6ffaa6bb0b408017b62254211691b5 |  24 |    0 |
    +----+----------+----------------------------------+-----+------+
    4 rows in set (0.01 sec)
    
    mysql> SELECT * FROM users4 ORDER BY age,id DESC LIMIT 2,4;        # 从结果集中显示第二条开始，输出四条记录
    +----+----------+----------------------------------+-----+------+
    | id | username | passwd                           | age | sex  |
    +----+----------+----------------------------------+-----+------+
    | 10 | wutian   | 900150983cd24fb0d6963f7d28e17f72 |  21 |    0 |
    |  9 | ZHANG    | 7f6ffaa6bb0b408017b62254211691b5 |  24 |    0 |
    |  3 | MAX      | 223                              |  24 |    0 |
    |  1 | JOHN     | 123                              |  24 |    0 |
    +----+----------+----------------------------------+-----+------+
    ```

    ```
    mysql> SELECT * FROM users4 LIMIT 2;
    +----+----------+--------+-----+------+
    | id | username | passwd | age | sex  |
    +----+----------+--------+-----+------+
    |  1 | JOHN     | 123    |  24 |    0 |
    |  2 | JOHN     | 123    |  27 |    0 |
    +----+----------+--------+-----+------+
    2 rows in set (0.00 sec)
    
    mysql> SELECT * FROM users4 LIMIT 2,3;        # id为1的记录的是第0 条记录
    +----+----------+--------+-----+------+
    | id | username | passwd | age | sex  |
    +----+----------+--------+-----+------+
    |  3 | MAX      | 223    |  24 |    0 |
    |  4 | JOHN     | 123    |  25 |    0 |
    |  5 | JOHN     | 123    |  18 |    0 |
    +----+----------+--------+-----+------+
    3 rows in set (0.00 sec)
    ```

    ```
    mysql> CREATE TABLE test(
        -> id TINYINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
        -> username VARCHAR(20)
        -> );
    Query OK, 0 rows affected (0.44 sec)
    
    mysql>INSERT test(username) SELECT username FROM users4 WHERE age >=22;
    Query OK, 6 rows affected (0.07 sec)
    Records: 6  Duplicates: 0  Warnings: 0
    
    mysql> SELECT * FROM test;
    +----+----------+
    | id | username |
    +----+----------+
    |  1 | JOHN     |
    |  2 | JOHN     |
    |  3 | MAX      |
    |  4 | JOHN     |
    |  5 | lINJIE   |
    |  6 | ZHANG    |
    +----+----------+
    6 rows in set (0.00 sec)
    ```

    ```
    mysql> SET NAMES UTF8;        # 设置在终端中显示的编码方式  不影响在数据库中的编码方式
    Query OK, 0 rows affected (0.00 sec)
    
    mysql> SELECT * FROM test;
    +----+----------+
    | id | username |
    +----+----------+
    |  1 | JOHN     |
    |  2 | JOHN     |
    |  3 | MAX      |
    |  4 | JOHN     |
    |  5 | lINJIE   |
    |  6 | ZHANG    |
    +----+----------+
    6 rows in set (0.00 sec)
    ```

    **注意：聚合函数，只有一个返回值**

    ```
    mysql> SELECT AVG (goods_price) FROM tdb_goods;        # 平均值
    +-------------------+
    | AVG (goods_price) |
    +-------------------+
    |      5636.3636364 |
    +-------------------+
    1 row in set (0.00 sec)
    
    mysql> SELECT ROUND(AVG(goods_price),2) FROM tdb_goods;        # 四舍五入
    +---------------------------+
    | ROUND(AVG(goods_price),2) |
    +---------------------------+
    |                   5636.36 |
    +---------------------------+
    1 row in set (0.00 sec)
    ```

    ```
    mysql> SELECT goods_id,goods_name,goods_price FROM tdb_goods WHERE goods_price > =5636.36; 
    +----------+-----------------------------------------+-------------+
    | goods_id | goods_name                              | goods_price |
    +----------+-----------------------------------------+-------------+
    |        3 | G150TH 15.6英寸游戏本                   |    8499.000 |
    |        7 | SVP13226SCB 13.3英寸触控超极本          |    7999.000 |
    |       13 | iMac ME086CH/A 21.5英寸一体电脑         |    9188.000 |
    |       17 | Mac Pro MD878CH/A 专业级台式电脑        |   28888.000 |
    |       18 |  HMZ-T3W 头戴显示设备                   |    6999.000 |
    |       20 | X3250 M4机架式服务器 2583i14            |    6888.000 |
    |       21 |  HMZ-T3W 头戴显示设备                   |    6999.000 |
    +----------+-----------------------------------------+-------------+
    7 rows in set (0.00 sec)
    
    mysql> SELECT goods_id,goods_name,goods_price FROM tdb_goods WHERE goods_price >
    =(SELECT ROUND(AVG(goods_price),2)FROM tdb_goods);            #  子查询
    +----------+-----------------------------------------+-------------+
    | goods_id | goods_name                              | goods_price |
    +----------+-----------------------------------------+-------------+
    |        3 | G150TH 15.6英寸游戏本                   |    8499.000 |
    |        7 | SVP13226SCB 13.3英寸触控超极本          |    7999.000 |
    |       13 | iMac ME086CH/A 21.5英寸一体电脑         |    9188.000 |
    |       17 | Mac Pro MD878CH/A 专业级台式电脑        |   28888.000 |
    |       18 |  HMZ-T3W 头戴显示设备                   |    6999.000 |
    |       20 | X3250 M4机架式服务器 2583i14            |    6888.000 |
    |       21 |  HMZ-T3W 头戴显示设备                   |    6999.000 |
    +----------+-----------------------------------------+-------------+
    7 rows in set (0.04 sec)
    ```

    - 使用外键处理重复的内容，防止表太大

    ```
    mysql> CREATE TABLE IF NOT EXISTS tdb_goods_cates(
        -> cate_id SMALLINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
        -> cate_name CARCHAR(40) NOT NULL
        -> );
    ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that
    corresponds to your MySQL server version for the right syntax to use near 'CARCH
    AR(40) NOT NULL
    )' at line 3
    mysql> CREATE TABLE IF NOT EXISTS tdb_goods_cates(
        -> cate_id SMALLINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
        -> cate_name VARCHAR(40) NOT NULL
        -> );
    Query OK, 0 rows affected (0.58 sec)
    
    mysql> SELECT goods_cate FROM tdb_goods GROUP BY goods_cate;
    +---------------------+
    | goods_cate          |
    +---------------------+
    | 台式机              |
    | 平板电脑            |
    | 服务器/工作站       |
    | 游戏本              |
    | 笔记本              |
    | 笔记本配件          |
    | 超级本              |
    +---------------------+
    7 rows in set (0.00 sec)
    
    mysql> INSERT tdb_goods_cates(cate_name)SELECT goods_cate FROM tdb_goods GROUP  BY goods_cate; 
    Query OK, 7 rows affected (0.13 sec)
    Records: 7  Duplicates: 0  Warnings: 0
    
    mysql> SELECT * FROM tdb_goods_cate;
    ERROR 1146 (42S02): Table 'muke.tdb_goods_cate' doesn't exist
    mysql> SELECT * FROM tdb_goods_cates;
    +---------+---------------------+
    | cate_id | cate_name           |
    +---------+---------------------+
    |       1 | 台式机              |
    |       2 | 平板电脑            |
    |       3 | 服务器/工作站       |
    |       4 | 游戏本              |
    |       5 | 笔记本              |
    |       6 | 笔记本配件          |
    |       7 | 超级本              |
    +---------+---------------------+
    ```

---

#### 参照表更新——多表更新

    - 表的连接

    ```
    mysql> UPDATE tdb_goods INNER JOIN tdb_goods_cates ON goods_cate = cate_name SET  goods_cate = cate_id;     # 内连接
    Query OK, 22 rows affected (0.07 sec)
    Rows matched: 22  Changed: 22  Warnings: 0
    
    mysql> SELECT * FROM tdb_goods\G;
    *************************** 1. row ***************************
       goods_id: 1
     goods_name: R510VC 15.6英寸笔记本
     goods_cate: 5
     brand_name: 华硕
    goods_price: 3399.000
        is_show: 1
     is_saleoff: 0
    *************************** 2. row ***************************
       goods_id: 2
     goods_name: Y400N 14.0英寸笔记本电脑
     goods_cate: 5
     brand_name: 联想
    goods_price: 4899.000
        is_show: 1
     is_saleoff: 0
    *************************** 3. row ***************************
       goods_id: 3
     goods_name: G150TH 15.6英寸游戏本
     goods_cate: 4
     brand_name: 雷神
    goods_price: 8499.000
        is_show: 1
     is_saleoff: 0
    *************************** 4. row ***************************
    ```

    - 多表更新一步到位:

    ```
    mysql> SELECT  brand_name FROM tdb_goods GROUP BY brand_name;
    +------------+
    | brand_name |
    +------------+
    | IBM        |
    | 华硕       |
    | 宏碁       |
    | 惠普       |
    | 戴尔       |
    | 索尼       |
    | 联想       |
    | 苹果       |
    | 雷神       |
    +------------+

    mysql> CREATE TABLE tdb_goods_brands(
        -> brand_id SMALLINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
        -> brand_name VARCHAR(40) NOT NULL
        -> )
        -> SELECT brand_name FROM tdb_goods GROUP BY brand_name;
    Query OK, 9 rows affected (0.68 sec)
    Records: 9  Duplicates: 0  Warnings: 0
    
    mysql> SELECT * FROM tdb_goods_brands;
    +----------+------------+
    | brand_id | brand_name |
    +----------+------------+
    |        1 | IBM        |
    |        2 | 华硕       |
    |        3 | 宏碁       |
    |        4 | 惠普       |
    |        5 | 戴尔       |
    |        6 | 索尼       |
    |        7 | 联想       |
    |        8 | 苹果       |
    |        9 | 雷神       |
    +----------+------------+
    9 rows in set (0.00 sec)
    ```

    ```
    mysql> UPDATE tdb_goods INNER JOIN tdb_goods_brands ON brand_name = brand_name S
    ET brand_name = brand_id
        -> ;
    ERROR 1052 (23000): Column 'brand_name' in field list is ambiguous    # 同名歧义
    mysql> UPDATE tdb_goods AS g INNER JOIN tdb_goods_brands AS b ON g.brand_name =b .brand_name SET g.brand_name = b.brand_id;             #  起别名 
    Query OK, 22 rows affected (0.07 sec)
    Rows matched: 22  Changed: 22  Warnings: 0
    
    mysql>
    
    其中表结构没有得到修改，尽量的修改   事实外键的使用，而尽量不使用物理外键
    mysql> ALTER TABLE tdb_goods
        -> CHANGE goods_cate cate_id SMALLINT UNSIGNED NOT NULL,
        -> CHANGE brand_name brands_id SMALLINT  UNSIGNED NOT NULL;
    Query OK, 22 rows affected (0.96 sec)
    Records: 22  Duplicates: 0  Warnings: 0
    
    mysql> SELECT * FROM tdb_goods\G;
    *************************** 1. row ***************************
       goods_id: 1
     goods_name: R510VC 15.6英寸笔记本
        cate_id: 5
      brands_id: 2
    goods_price: 3399.000
        is_show: 1
     is_saleoff: 0
    *************************** 2. row ***************************
       goods_id: 2
     goods_name: Y400N 14.0英寸笔记本电脑
        cate_id: 5
      brands_id: 7
    goods_price: 4899.000
        is_show: 1
     is_saleoff: 0
    *************************** 3. row ***************************
    
    mysql> SELECT goods_id ,goods_name,cate_name,brand_name,goods_price FROM tdb_goo
    ds AS g
        -> INNER JOIN tdb_goods_cates AS c ON g.cate_id = c.cate_id
        -> INNER JOIN tdb_goods_brands AS b ON g.brands_id= b.brand_id \G;
    *************************** 1. row ***************************
       goods_id: 1
     goods_name: R510VC 15.6英寸笔记本
      cate_name: 笔记本
     brand_name: 华硕
    goods_price: 3399.000
    *************************** 2. row ***************************
       goods_id: 2
     goods_name: Y400N 14.0英寸笔记本电脑
      cate_name: 笔记本
     brand_name: 联想
    goods_price: 4899.000
    *************************** 3. row ***************************
       goods_id: 3
     goods_name: G150TH 15.6英寸游戏本
      cate_name: 游戏本
     brand_name: 雷神
    goods_price: 8499.000
    *************************** 4. row ***************************
       goods_id: 4
     goods_name: X550CC 15.6英寸笔记本
      cate_name: 笔记本
     brand_name: 华硕
    goods_price: 2799.000
    *************************** 5. row ***************************
       goods_id: 5
     goods_name: X240(20ALA0EYCD) 12.5英寸超极本
      cate_name: 超级本
     brand_name: 联想
    goods_price: 4999.000
    *************************** 6. row ***************************
    ```

    - 无限分类的数据表设计 （自连接）

    ```
       CREATE TABLE tdb_goods_types(
         type_id   SMALLINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
         type_name VARCHAR(20) NOT NULL,
         parent_id SMALLINT UNSIGNED NOT NULL DEFAULT 0
      );
    
      INSERT tdb_goods_types(type_name,parent_id) VALUES('家用电器',DEFAULT);
      INSERT tdb_goods_types(type_name,parent_id) VALUES('电脑、办公',DEFAULT);
      INSERT tdb_goods_types(type_name,parent_id) VALUES('大家电',1);
      INSERT tdb_goods_types(type_name,parent_id) VALUES('生活电器',1);
      INSERT tdb_goods_types(type_name,parent_id) VALUES('平板电视',3);
      INSERT tdb_goods_types(type_name,parent_id) VALUES('空调',3);
      INSERT tdb_goods_types(type_name,parent_id) VALUES('电风扇',4);
      INSERT tdb_goods_types(type_name,parent_id) VALUES('饮水机',4);
      INSERT tdb_goods_types(type_name,parent_id) VALUES('电脑整机',2);
      INSERT tdb_goods_types(type_name,parent_id) VALUES('电脑配件',2);
      INSERT tdb_goods_types(type_name,parent_id) VALUES('笔记本',9);
      INSERT tdb_goods_types(type_name,parent_id) VALUES('超级本',9);
      INSERT tdb_goods_types(type_name,parent_id) VALUES('游戏本',9);
      INSERT tdb_goods_types(type_name,parent_id) VALUES('CPU',10);
      INSERT tdb_goods_types(type_name,parent_id) VALUES('主机',10);
    
    mysql> SELECT * FROM   tdb_goods_types;
    +---------+-----------------+-----------+
    | type_id | type_name       | parent_id |
    +---------+-----------------+-----------+
    |       1 | 家用电器        |         0 |
    |       2 | 电脑、办公      |         0 |
    |       3 | 大家电          |         1 |
    |       4 | 生活电器        |         1 |
    |       5 | 平板电视        |         3 |
    |       6 | 空调            |         3 |
    |       7 | 电风扇          |         4 |
    |       8 | 饮水机          |         4 |
    |       9 | 电脑整机        |         2 |
    |      10 | 电脑配件        |         2 |
    |      11 | 笔记本          |         9 |
    |      12 | 超级本          |         9 |
    |      13 | 游戏本          |         9 |
    |      14 | CPU             |        10 |
    |      15 | 主机            |        10 |
    +---------+-----------------+-----------+
    15 rows in set (0.00 sec)
    
    mysql>
    
    mysql> SELECT s.type_id,s.type_name,p.type_name FROM tdb_goods_types AS s LEFT JOIN tdb_goods_types AS p ON s.parent_id = p.type_id; 
    +---------+-----------------+-----------------+
    | type_id | type_name       | type_name       |
    +---------+-----------------+-----------------+
    |       1 | 家用电器        | NULL            |
    |       2 | 电脑、办公      | NULL            |
    |       3 | 大家电          | 家用电器        |
    |       4 | 生活电器        | 家用电器        |
    |       5 | 平板电视        | 大家电          |
    |       6 | 空调            | 大家电          |
    |       7 | 电风扇          | 生活电器        |
    |       8 | 饮水机          | 生活电器        |
    |       9 | 电脑整机        | 电脑、办公      |
    |      10 | 电脑配件        | 电脑、办公      |
    |      11 | 笔记本          | 电脑整机        |
    |      12 | 超级本          | 电脑整机        |
    |      13 | 游戏本          | 电脑整机        |
    |      14 | CPU             | 电脑配件        |
    |      15 | 主机            | 电脑配件        |
    +---------+-----------------+-----------------+
    15 rows in set (0.00 sec)
    ```

    ```
    mysql> SELECT p.type_id,p.type_name,s.type_name FROM tdb_goods_types p LEFT JOIN
     tdb_goods_types s ON s.parent_id = p.type_id;        # 缺省AS 
    +---------+-----------------+--------------+
    | type_id | type_name       | type_name    |
    +---------+-----------------+--------------+
    |       1 | 家用电器        | 大家电       |
    |       1 | 家用电器        | 生活电器     |
    |       3 | 大家电          | 平板电视     |
    |       3 | 大家电          | 空调         |
    |       4 | 生活电器        | 电风扇       |
    |       4 | 生活电器        | 饮水机       |
    |       2 | 电脑、办公      | 电脑整机     |
    |       2 | 电脑、办公      | 电脑配件     |
    |       9 | 电脑整机        | 笔记本       |
    |       9 | 电脑整机        | 超级本       |
    |       9 | 电脑整机        | 游戏本       |
    |      10 | 电脑配件        | CPU          |
    |      10 | 电脑配件        | 主机         |
    |       5 | 平板电视        | NULL         |
    |       6 | 空调            | NULL         |
    |       7 | 电风扇          | NULL         |
    |       8 | 饮水机          | NULL         |
    |      11 | 笔记本          | NULL         |
    |      12 | 超级本          | NULL         |
    |      13 | 游戏本          | NULL         |
    |      14 | CPU             | NULL         |
    |      15 | 主机            | NULL         |
    +---------+-----------------+--------------+
    
    ```
    
    mysql> SELECT p.type_id,p.type_name,s.type_name FROM tdb_goods_types p LEFT JOIN
     tdb_goods_types s ON s.parent_id = p.type_id GROUP BY p.type_id;
    +---------+-----------------+--------------+
    | type_id | type_name       | type_name    |
    +---------+-----------------+--------------+
    |       1 | 家用电器        | 大家电       |
    |       2 | 电脑、办公      | 电脑整机     |
    |       3 | 大家电          | 平板电视     |
    |       4 | 生活电器        | 电风扇       |
    |       5 | 平板电视        | NULL         |
    |       6 | 空调            | NULL         |
    |       7 | 电风扇          | NULL         |
    |       8 | 饮水机          | NULL         |
    |       9 | 电脑整机        | 笔记本       |
    |      10 | 电脑配件        | CPU          |
    |      11 | 笔记本          | NULL         |
    |      12 | 超级本          | NULL         |
    |      13 | 游戏本          | NULL         |
    |      14 | CPU             | NULL         |
    |      15 | 主机            | NULL         |
    +---------+-----------------+--------------+
    15 rows in set (0.01 sec)
    
    mysql>
    mysql> SELECT p.type_id,p.type_name,count(s.type_name) child_count FROM tdb_good s_types p LEFT JOIN tdb_goods_types s ON s.parent_id = p.type_id GROUP BY p.type _id; 
    +---------+-----------------+-------------+
    | type_id | type_name       | child_count |
    +---------+-----------------+-------------+
    |       1 | 家用电器        |           2 |
    |       2 | 电脑、办公      |           2 |
    |       3 | 大家电          |           2 |
    |       4 | 生活电器        |           2 |
    |       5 | 平板电视        |           0 |
    |       6 | 空调            |           0 |
    |       7 | 电风扇          |           0 |
    |       8 | 饮水机          |           0 |
    |       9 | 电脑整机        |           3 |
    |      10 | 电脑配件        |           2 |
    |      11 | 笔记本          |           0 |
    |      12 | 超级本          |           0 |
    |      13 | 游戏本          |           0 |
    |      14 | CPU             |           0 |
    |      15 | 主机            |           0 |
    +---------+-----------------+-------------+
    15 rows in set (0.00 sec)
    ```



    - 多表删除

      删除重复记录，保留ID号最小的记录（自身连接）

    ```
    mysql> DELETE t1 FROM tdb_goods AS t1 LEFT JOIN (SELECT goods_id,goods_name FROM
    tdb_goods GROUP BY goods_name HAVING count(goods_name)>=2) AS t2 ON t1.goods_nam
    e = t2.goods_name WHERE t1.goods_id >t2.goods_id;
    Query OK, 2 rows affected (0.12 sec)
    ```
        
---

#### MySQL函数

    - 字符函数

    ```
    mysql> SELECT CONCAT('LINJIE','MYSQL');
    +--------------------------+
    | CONCAT('LINJIE','MYSQL') |
    +--------------------------+
    | LINJIEMYSQL              |
    +--------------------------+
    1 row in set (0.01 sec)
    ```

    ```
    mysql> SELECT CONCAT('LINJIE','-','MYSQL');
    +------------------------------+
    | CONCAT('LINJIE','-','MYSQL') |
    +------------------------------+
    | LINJIE-MYSQL                 |
    +------------------------------+
    ```

    ```
    mysql> select * from test;
    +----+----------+
    | id | username |
    +----+----------+
    |  1 | JOHN     |
    |  2 | JOHN     |
    |  3 | MAX      |
    |  4 | JOHN     |
    |  5 | lINJIE   |
    |  6 | ZHANG    |
    +----+----------+
    ```

    ```
    mysql> SELECT CONCAT(id,username) AS FULLNAME FROM TEST;
    +----------+
    | FULLNAME |
    +----------+
    | 1JOHN    |
    | 2JOHN    |
    | 3MAX     |
    | 4JOHN    |
    | 5lINJIE  |
    | 6ZHANG   |
    +----------+
    ```

    ```
    mysql> SELECT CONCAT(id,'-',username) AS FULLNAME FROM TEST;
    +----------+
    | FULLNAME |
    +----------+
    | 1-JOHN   |
    | 2-JOHN   |
    | 3-MAX    |
    | 4-JOHN   |
    | 5-lINJIE |
    | 6-ZHANG  |
    +----------+
    ```

    ```
    mysql> SELECT CONCAT_WS('-','LINJIE','GOOD','FUNCTIONS');
    +--------------------------------------------+
    | CONCAT_WS('-','LINJIE','GOOD','FUNCTIONS') |
    +--------------------------------------------+
    | LINJIE-GOOD-FUNCTIONS                      |
    +--------------------------------------------+
    1 row in set (0.00 sec)
    ```

    ```
    mysql> SELECT FORMAT(123456.77,2);
    +---------------------+
    | FORMAT(123456.77,2) |
    +---------------------+
    | 123,456.77          |
    +---------------------+
    1 row in set (0.01 sec)
    ```

    ```
    mysql> SELECT FORMAT(123456.778,2);
    +----------------------+
    | FORMAT(123456.778,2) |
    +----------------------+
    | 123,456.78           |
    +----------------------+
    ```

    ```
    mysql>
    mysql> SELECT LOWER('MYSQl');
    +----------------+
    | LOWER('MYSQl') |
    +----------------+
    | mysql          |
    +----------------+
    ```

    ```
    mysql> SELECT UPPER('Mysql');
    +----------------+
    | UPPER('Mysql') |
    +----------------+
    | MYSQL          |
    +----------------+
    ```

    ```
    mysql> SELECT LEFT ('mYSQL',2);
    +------------------+
    | LEFT ('mYSQL',2) |
    +------------------+
    | mY               |                    # 取出前面指定个数的字符
    +------------------+
    ```

    ```    
    mysql> SELECT LOWER(LEFT('MYSQL',2));
    +------------------------+
    | LOWER(LEFT('MYSQL',2)) |
    +------------------------+
    | my                     |
    +------------------------+
    ```

    ```
    mysql> SELECT LOWER(RIGHT('MYSQL',2));
    +-------------------------+
    | LOWER(RIGHT('MYSQL',2)) |
    +-------------------------+
    | ql                      |
    +-------------------------+
    ```

    ```
    mysql> SELECT LENGTH('MYSQL L  ');        # 求字符串长度
    +---------------------+
    | LENGTH('MYSQL L  ') |
    +---------------------+
    |                   9 |
    +---------------------+
    ```

    ```
    mysql> SELECT LENGTH('MYSQL L      ');
    +-------------------------+
    | LENGTH('MYSQL L      ') |
    +-------------------------+
    |                      13 |
    +-------------------------+
    ```

    ```
    mysql> SELECT LTRIM('  MYSQL LINJIE   ');        # 删除前导空格
    +----------------------------+
    | LTRIM('  MYSQL LINJIE   ') |
    +----------------------------+
    | MYSQL LINJIE               |
    +----------------------------+
    ```


    ```
    mysql> SELECT LENGTH( LTRIM('  MYSQL LINJIE   '));      
    +-------------------------------------+
    | LENGTH( LTRIM('  MYSQL LINJIE   ')) |
    +-------------------------------------+
    |                                  15 |
    +-------------------------------------+
    ```

    ```
    mysql> SELECT RTRIM('  MYSQL LINJIE   ');
    +----------------------------+
    | RTRIM('  MYSQL LINJIE   ') |
    +----------------------------+
    |   MYSQL LINJIE             |
    +----------------------------+
    ```

    ```
    mysql> SELECT LENGTH( RTRIM('  MYSQL LINJIE   '));
    +-------------------------------------+
    | LENGTH( RTRIM('  MYSQL LINJIE   ')) |
    +-------------------------------------+
    |                                  14 |
    +-------------------------------------+
    ```

    ```
    mysql> SELECT LENGTH(TRIM('  MYSQL LINJIE   '));
    +-----------------------------------+
    | LENGTH(TRIM('  MYSQL LINJIE   ')) |
    +-----------------------------------+
    |                                12 |
    +-----------------------------------+
    ```

    ```
    mysql> SELECT TRIM(LEADING '?' FROM '???MYSQL???');        # 删除前导的？
    +--------------------------------------+
    | TRIM(LEADING '?' FROM '???MYSQL???') |
    +--------------------------------------+
    | MYSQL???                             |
    +--------------------------------------+
    1 row in set (0.00 sec)
    ```

    ```
    mysql> SELECT TRIM(TRAILING '?' FROM '???MYSQL???');        # 删除后导字符？
    +---------------------------------------+
    | TRIM(TRAILING '?' FROM '???MYSQL???') |
    +---------------------------------------+
    | ???MYSQL                              |
    +---------------------------------------+
    1 row in set (0.00 sec)
    ```

    ```
    mysql> SELECT TRIM(BOTH '?' FROM '???MYSQL???');        # 删除字符串中的？字符
    +-----------------------------------+
    | TRIM(BOTH '?' FROM '???MYSQL???') |
    +-----------------------------------+
    | MYSQL                             |
    +-----------------------------------+
    1 row in set (0.00 sec)
    ```

    ```
    mysql> SELECT REPLACE ('???MYS??QL??','?','');
    +---------------------------------+
    | REPLACE ('???MYS??QL??','?','') |
    +---------------------------------+
    | MYSQL                           |
    +---------------------------------+
    ```

    ```
    mysql> SELECT REPLACE ('???MYS??QL??','?','!');
    +----------------------------------+
    | REPLACE ('???MYS??QL??','?','!') |
    +----------------------------------+
    | !!!MYS!!QL!!                     |
    +----------------------------------+
    ```

    ```
    mysql> SELECT SUBSTRING('MYSQL',1,2);    # 截取字符
    +------------------------+
    | SUBSTRING('MYSQL',1,2) |
    +------------------------+
    | MY                     |
    +------------------------+
    ```

    ```
    mysql> SELECT SUBSTRING('MYSQL',3);        # 截取字符从第三位开始到家结束
    +----------------------+
    | SUBSTRING('MYSQL',3) |
    +----------------------+
    | SQL                  |
    +----------------------+
    ```

    ```
    mysql> SELECT SUBSTRING('LINJIE',3);
    +-----------------------+
    | SUBSTRING('LINJIE',3) |
    +-----------------------+
    | NJIE                  |
    +-----------------------+
    1 row in set (0.00 sec)
    ```

    ```
    mysql> SELECT SUBSTRING('LINJIE',-3);    # 倒数三位
    +------------------------+
    | SUBSTRING('LINJIE',-3) |
    +------------------------+
    | JIE                    |
    +------------------------+
    ```

    ```
    mysql> SELECT 'MYSQL' LINKE 'M%';
    ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that
    corresponds to your MySQL server version for the right syntax to use near ''M%''
     at line 1
    mysql> SELECT 'MYSQL' LIKE 'M%';
    +-------------------+
    | 'MYSQL' LIKE 'M%' |
    +-------------------+
    |                 1 |
    +-------------------+
    ```

    ```
    mysql> SELECT  * FROM test;
    +----+----------+
    | id | username |
    +----+----------+
    |  1 | JOHN     |
    |  2 | JOHN     |
    |  3 | MAX      |
    |  4 | JOHN     |
    |  5 | lINJIE   |
    |  6 | ZHANG    |
    +----+----------+
    ```

    ```
    mysql> INSERT INTO test VALUES(NULL,'TOM%');
    Query OK, 1 row affected (0.10 sec)
    
    mysql> SELECT  * FROM test;
    +----+----------+
    | id | username |
    +----+----------+
    |  1 | JOHN     |
    |  2 | JOHN     |
    |  3 | MAX      |
    |  4 | JOHN     |
    |  5 | lINJIE   |
    |  6 | ZHANG    |
    |  8 | TOM%     |
    +----+----------+
    ```

    ```
    mysql> SELECT * FROM test WHERE username LIKE '%%%';
    +----+----------+
    | id | username |
    +----+----------+
    |  1 | JOHN     |
    |  2 | JOHN     |
    |  3 | MAX      |
    |  4 | JOHN     |
    |  5 | lINJIE   |
    |  6 | ZHANG    |
    |  8 | TOM%     |
    +----+----------+
    ```

    ```
    mysql> SELECT * FROM test WHERE username LIKE '%1%%' ESCAPE 1;        # % 是通配符 忽略数字1 后面的%当做符号%来使用 而不是通配符
    +----+----------+
    | id | username |
    +----+----------+
    |  8 | TOM%     |
    +----+----------+
    1 row in set (0.00 sec)
    ```

---

#### 数值运算

    ```
    mysql> SELECT 2+7;
    +-----+
    | 2+7 |
    +-----+
    |   9 |
    +-----+
    1 row in set (0.00 sec)
        
    mysql> SELECT CEIL(3.01);    #向上取整
    +------------+
    | CEIL(3.01) |
    +------------+
    |          4 |
    +------------+
    1 row in set (0.00 sec)
        
    mysql> SELECT FLOOR(3.01);        # 向下取整
    +-------------+
    | FLOOR(3.01) |
    +-------------+
    |           3 |
    +-------------+
    1 row in set (0.00 sec)
    
    ```

    ```
    mysql> SELECT 3 DIV 4;
    +---------+
    | 3 DIV 4 |
    +---------+
    |       0 |
    +---------+
    1 row in set (0.01 sec)
    
    mysql> SELECT 5 DIV 4;
    +---------+
    | 5 DIV 4 |
    +---------+
    |       1 |
    +---------+
    1 row in set (0.00 sec)
    
    mysql> SELECT 5 MOD 4;
    +---------+
    | 5 MOD 4 |
    +---------+
    |       1 |
    +---------+
    1 row in set (0.00 sec)
    
    mysql> SELECT 5 MOD 2.3;
    +-----------+
    | 5 MOD 2.3 |
    +-----------+
    |       0.4 |
    +-----------+
    1 row in set (0.00 sec)
    
    mysql> SELECT POWER(3,3);
    +------------+
    | POWER(3,3) |
    +------------+
    |         27 |
    +------------+
    1 row in set (0.00 sec)
    
    mysql> SELECT ROUND(3.6545,3);
    +-----------------+
    | ROUND(3.6545,3) |
    +-----------------+
    |           3.655 |
    +-----------------+
    1 row in set (0.00 sec)
    
    mysql> SELECT TRUNCATE(3.6545,3);
    +--------------------+
    | TRUNCATE(3.6545,3) |
    +--------------------+
    |              3.654 |
    +--------------------+
    1 row in set (0.00 sec)
    
    mysql>
    
    mysql> SELECT 15 BETWEEN 1 AND 22;
    +---------------------+
    | 15 BETWEEN 1 AND 22 |
    +---------------------+
    |                   1 |
    +---------------------+
    1 row in set (0.01 sec)
    
    mysql> SELECT 35 BETWEEN 1 AND 22;
    +---------------------+
    | 35 BETWEEN 1 AND 22 |
    +---------------------+
    |                   0 |
    +---------------------+
    1 row in set (0.00 sec)
    
    mysql> SELECT 35 NOT BETWEEN 1 AND 22;
    +-------------------------+
    | 35 NOT BETWEEN 1 AND 22 |
    +-------------------------+
    |                       1 |
    +-------------------------+
    1 row in set (0.00 sec)
    
    mysql> SELECT 35 IN(15,25,35);
    +-----------------+
    | 35 IN(15,25,35) |
    +-----------------+
    |               1 |
    +-----------------+
    1 row in set (0.00 sec)
    
    mysql> SELECT 30 IN(15,25,35);
    +-----------------+
    | 30 IN(15,25,35) |
    +-----------------+
    |               0 |
    +-----------------+
    1 row in set (0.00 sec)
    
    mysql> SELECT NULL IS NULL;
    +--------------+
    | NULL IS NULL |
    +--------------+
    |            1 |
    +--------------+
    1 row in set (0.00 sec)
    
    mysql> SELECT NULL IS NOT NULL;
    +------------------+
    | NULL IS NOT NULL |
    +------------------+
    |                0 |
    +------------------+
    1 row in set (0.00 sec)
    
    mysql> SELECT '' IS NOT NULL;
    +----------------+
    | '' IS NOT NULL |
    +----------------+
    |              1 |
    +----------------+
    1 row in set (0.00 sec)
    
    mysql> SELECT '' IS NULL;
    +------------+
    | '' IS NULL |
    +------------+
    |          0 |
    +------------+
    1 row in set (0.00 sec)
    ```


---

#### 日期时间函数

    ```
    mysql> SELECT NOW();
    +---------------------+
    | NOW()               |
    +---------------------+
    | 2015-04-06 22:20:33 |
    +---------------------+
    1 row in set (0.00 sec)
    
    mysql> SELECT CURDATE();
    +------------+
    | CURDATE()  |
    +------------+
    | 2015-04-06 |
    +------------+
    1 row in set (0.00 sec)
    
    mysql> SELECT CURTIME();
    +-----------+
    | CURTIME() |
    +-----------+
    | 22:20:53  |
    +-----------+
    1 row in set (0.00 sec)
    
    mysql> SELECT DATE_ADD('2015-4-6',INTERVAL 365 DAY);
    +---------------------------------------+
    | DATE_ADD('2015-4-6',INTERVAL 365 DAY) |
    +---------------------------------------+
    | 2016-04-05                            |
    +---------------------------------------+
    1 row in set (0.00 sec)
    
    mysql> SELECT DATE_ADD('2015-4-6',INTERVAL -365 DAY);
    +----------------------------------------+
    | DATE_ADD('2015-4-6',INTERVAL -365 DAY) |
    +----------------------------------------+
    | 2014-04-06                             |
    +----------------------------------------+
    1 row in set (0.00 sec)
    
    mysql> SELECT DATE_ADD('2015-4-6',INTERVAL 1 YEAR);
    +--------------------------------------+
    | DATE_ADD('2015-4-6',INTERVAL 1 YEAR) |
    +--------------------------------------+
    | 2016-04-06                           |
    +--------------------------------------+
    1 row in set (0.00 sec)
    
    mysql> SELECT DATE_ADD('2015-4-6',INTERVAL 1 WEEK);        # 添加一个星期
    +--------------------------------------+
    | DATE_ADD('2015-4-6',INTERVAL 1 WEEK) |
    +--------------------------------------+
    | 2015-04-13                           |
    +--------------------------------------+
    1 row in set (0.00 sec)
    
    mysql> SELECT DATE_ADD('2015-4-6',INTERVAL 1 WEEK);
    +--------------------------------------+
    | DATE_ADD('2015-4-6',INTERVAL 1 WEEK) |
    +--------------------------------------+
    | 2015-04-13                           |
    +--------------------------------------+
    1 row in set (0.00 sec)
    
    mysql> SELECT DATEDIFF('2014-1-1','2015-2-2');
    +---------------------------------+
    | DATEDIFF('2014-1-1','2015-2-2') |
    +---------------------------------+
    |                            -397 |
    +---------------------------------+
    1 row in set (0.00 sec)
    
    
    mysql> SELECT DATE_FORMAT('2015-4-6','%M/%D/%Y');
    +------------------------------------+
    | DATE_FORMAT('2015-4-6','%M/%D/%Y') |
    +------------------------------------+
    | April/6th/2015                     |
    +------------------------------------+
    1 row in set (0.01 sec)
    ```

---

#### 信息函数

    ```
    mysql> SELECT CONNECTION_ID();
    +-----------------+
    | CONNECTION_ID() |
    +-----------------+
    |               3 |
    +-----------------+
    1 row in set (0.00 sec)
    
    mysql> SELECT DATABASE();
    +------------+
    | DATABASE() |
    +------------+
    | muke       |
    +------------+
    1 row in set (0.01 sec)
    
    mysql> SELECT USER();
    +----------------+
    | USER()         |
    +----------------+
    | root@localhost |
    +----------------+
    1 row in set (0.00 sec)
    
    mysql> SELECT VERSION();
    +------------+
    | VERSION()  |
    +------------+
    | 5.6.21-log |
    +------------+
    1 row in set (0.00 sec)
    ```

    ```
    mysql> SELECT MD5('123');
    +----------------------------------+
    | MD5('123')                       |
    +----------------------------------+
    | 202cb962ac59075b964b07152d234b70 |
    +----------------------------------+
    1 row in set (0.00 sec)
    
    mysql> SELECT PASSWORD('123');
    +-------------------------------------------+
    | PASSWORD('123')                           |
    +-------------------------------------------+
    | *23AE809DDACAF96AF0FD78ED04B6A265E05AA257 |
    +-------------------------------------------+
    1 row in set (0.00 sec)
    
    mysql> SET PASSWORD = PASSWORD('linjie520');        # 修改当前用户的密码
    Query OK, 0 rows affected (0.16 sec)
    ```

----

#### MySQL自定义函数
```
mysql> CREATE FUNCTION f1() RETURNS VARCHAR(30)        # 定义函数
    -> RETURN DATE_FORMAT(NOW(),'%Y年%m月%d日 %h点：%i分：%s秒')
    -> ;
Query OK, 0 rows affected (0.01 sec)

mysql> SELECT f1();        # 调用函数
+---------------------------------+
| f1()                            |
+---------------------------------+
| 2015年04月06日 10点：57分：10秒 |
+---------------------------------+
1 row in set (0.04 sec)
```

```
mysql> CREATE FUNCTION f2(num1 SMALLINT UNSIGNED,num2 SMALLINT UNSIGNED)
    -> RETURNS FLOAT(10,2) UNSIGNED
    -> RETURN (num1+num2)/2;
Query OK, 0 rows affected (0.00 sec)

mysql> SELECT f2(10,69);
+-----------+
| f2(10,69) |
+-----------+
|     39.50 |
+-----------+
1 row in set (0.00 sec)
```

```
mysql> DELIMITER //        # 定义语句结束符 避免在写函数的时候的分号冲突
mysql>
mysql> CREATE FUNCTION adduser(username VARCHAR(20))
    -> RETURNS INT UNSIGNED
    -> BEGIN
    -> INSERT test (username) VALUES(username);
    -> RETURN LAST_INSERT_ID();
    -> END        # 和 BEGIN 构成聚合体
    -> //
Query OK, 0 rows affected (0.00 sec)

mysql> SELECT adduser('bubu');
    -> //
+-----------------+
| adduser('bubu') |
+-----------------+
|               9 |
+-----------------+
1 row in set (0.06 sec)
```

```
mysql> select * from test//
+----+----------+
| id | username |
+----+----------+
|  1 | JOHN     |
|  2 | JOHN     |
|  3 | MAX      |
|  4 | JOHN     |
|  5 | lINJIE   |
|  6 | ZHANG    |
|  8 | TOM%     |
|  9 | bubu     |
+----+----------+
8 rows in set (0.00 sec)

mysql> DROP FUNCTION adduser; //    # 删除自定义函数
```



------


#### 存储过程

- 默认当前用户创建存储过程（过程体）

- 增删改查，多表连接操作的存储过程！！！


```
root@localhost muke>CREATE PROCEDURE sp1() SELECT VERSION();
Query OK, 0 rows affected (0.10 sec)

root@localhost muke>CALL sp1;
+------------+
| VERSION()  |
+------------+
| 5.6.21-log |
+------------+
1 row in set (0.00 sec)

Query OK, 0 rows affected (0.01 sec)
```


```
root@localhost muke>CALL sp1();
+------------+
| VERSION()  |
+------------+
| 5.6.21-log |
+------------+
1 row in set (0.00 sec)

Query OK, 0 rows affected (0.01 sec)

root@localhost muke>DELIMITER //

root@localhost muke>CREATE PROCEDURE removeGoodsId(IN id INT UNSIGNED)
    -> BEGIN
    -> DELETE FROM tdb_goods WHERE id = id;        # 这里的参数和表字段同名 导致删除的时候所有的表内容都删除了
    -> END
    -> //
Query OK, 0 rows affected (0.00 sec)                              注意： 这里的IN id要修改为其他形参，再修改 WHERE id = 形参
```


```
root@localhost muke>DELIMITER ;
root@localhost muke>CALL removeGoodsId(3);
Query OK, 20 rows affected (0.07 sec)

root@localhost muke>SELECT * FROM tdb_goods;
Empty set (0.00 sec)

```

- 存储过程书写错误的时候要删除后重建！！！


```
root@localhost muke>DROP PROCEDURE removeGoodsId;
Query OK, 0 rows affected (0.00 sec)    
root@localhost muke>DELIMITER //
root@localhost muke>CREATE PROCEDURE removeUserAndReturnUserNums(IN P_ID INT UNSIGNED,OUT userNums INT UNSIGNED)
    -> BEGIN
    -> DELETE FROM users WHERE id = p_id;
    -> SELECT count(id) FROM users INTO userNums;        #  局部变量声明
    -> END
    -> //
Query OK, 0 rows affected (0.00 sec)

root@localhost muke>DELIMITER ;
root@localhost muke>CALL removeUserAndReturnUserNums(4,@nums);
Query OK, 1 row affected (0.06 sec)

root@localhost muke>SELECT * FROM users;
+----+----------+------+
| id | username | pid  |
+----+----------+------+
|  3 | LINJIE   | NULL |
|  5 | lin      | NULL |
|  6 | uunh     | NULL |
|  7 | MAX      | NULL |
+----+----------+------+
4 rows in set (0.00 sec)

root@localhost muke>SELECT @nums ;        #  
+-------+
| @nums |
+-------+
|     4 |
+-------+
1 row in set (0.00 sec)
```


```
root@localhost muke>SELECT ROW_COUNT();        # 得到插入、删除、更新、被影响的行数
+-------------+
| ROW_COUNT() |
+-------------+
|          -1 |
+-------------+
1 row in set (0.00 sec)
```

**存储过程会很复杂用来对表进行操作，函数的针对性更强些而很少用函数对表操作。**

-----


#### MySQL锁策略：

    ```
    mysql> CREATE TABLE tb5(
        -> s1 VARCHAR(10)
        -> ) ENGINE = MyISAM;	# setting enging
    Query OK, 0 rows affected (0.10 sec)
    
    mysql> SHOW CREATE TABLE tb5;
    | tb5   | CREATE TABLE `tb5` (
      `s1` varchar(10) DEFAULT NULL
    ) ENGINE=MyISAM DEFAULT CHARSET=utf8 |
    
    mysql> ALTER TABLE tb5 ENGINE =InnoDB;
    Query OK, 0 rows affected (0.52 sec)
    Records: 0  Duplicates: 0  Warnings: 0
    
    mysql> SHOW CREATE TABLE tb5;
    | tb5   | CREATE TABLE `tb5` (
      `s1` varchar(10) DEFAULT NULL
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8 |
    
    str_to_date("2015-05-06", "%y-%m-%d")  日期处理
    ```

    ```
    UPDATE supply_chain_stats_daily SET return_num=0
    WHERE   created BETWEEN '2015-09-17' AND '2015-09-19';
    
    Error Code: 1175. You are using safe update mode and you tried to
     update a table without a WHERE that uses a KEY column To disable safe mode, 
     toggle the option in Preferences -> SQL Editor and reconnect.
    将 SET SQL_SAFE_UPDATES = 1;  # 当前会话有效
    设置为： SET SQL_SAFE_UPDATES = 0;
    再使用更新语句。
    ```
    

