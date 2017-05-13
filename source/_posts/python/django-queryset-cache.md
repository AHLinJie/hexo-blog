---
layout: post
title: django queryset cache
date: 2016-02-05
category: python
tag: django-queryset
---
**摘要:**
我们在使用django的orm的时候很顺手,但是要注意一下这里的queryset的cache情况,这里
做下笔记.


1. 数组切片或者指定index取值的时候是不会使用cache的
```
In [2]: queryset = GovHostInfo.objects.all()
In [3]: print queryset[5]
(0.000) SELECT @@SQL_AUTO_IS_NULL; args=None
(0.000) SELECT `gov_host_info`.`id`, `gov_host_info`.`created`, `gov_host_info`.`modified`, `gov_host_info`.`name`, `gov_host_info`.`host_url`, `gov_host_info`.`purchase_path`, `gov_host_info`.`is_open`, `gov_host_info`.`gov_level`, `gov_host_info`.`memo` FROM `gov_host_info` LIMIT 1 OFFSET 5; args=()
10-金安区人民政府网

In [4]: print queryset[3]
(0.000) SELECT `gov_host_info`.`id`, `gov_host_info`.`created`, `gov_host_info`.`modified`, `gov_host_info`.`name`, `gov_host_info`.`host_url`, `gov_host_info`.`purchase_path`, `gov_host_info`.`is_open`, `gov_host_info`.`gov_level`, `gov_host_info`.`memo` FROM `gov_host_info` LIMIT 1 OFFSET 3; args=()
8-霍邱县人民政府网
```
2. 第一次遍历的时候不会使用cache
```
In [2]: queryset = GovHostInfo.objects.all()
In [5]: print [g for g in queryset]
(0.001) SELECT `gov_host_info`.`id`, `gov_host_info`.`created`, `gov_host_info`.`modified`, `gov_host_info`.`name`, `gov_host_info`.`host_url`, `gov_host_info`.`purchase_path`, `gov_host_info`.`is_open`, `gov_host_info`.`gov_level`, `gov_host_info`.`memo` FROM `gov_host_info`; args=()
[<GovHostInfo: 5-舒城县人民政府网>, <GovHostInfo: 6-霍山县人民政府网>, <GovHostInfo: 7-寿县人民政府网>, <GovHostInfo: 8-霍邱县人民政府网>, <GovHostInfo: 9-金寨县人民政府网>, <GovHostInfo: 10-金安区人民政府网>, <GovHostInfo: 11-裕安区人民政府网>, <GovHostInfo: 12-金安区政府采购网>]

In [6]: print [g for g in queryset]
[<GovHostInfo: 5-舒城县人民政府网>, <GovHostInfo: 6-霍山县人民政府网>, <GovHostInfo: 7-寿县人民政府网>, <GovHostInfo: 8-霍邱县人民政府网>, <GovHostInfo: 9-金寨县人民政府网>, <GovHostInfo: 10-金安区人民政府网>, <GovHostInfo: 11-裕安区人民政府网>, <GovHostInfo: 12-金安区政府采购网>]

In [7]: print queryset[3]  # 注意这里 是从cache中获取的
8-霍邱县人民政府网
```
3. if会触发执行sql
```
In [14]: queryset = GovHostInfo.objects.all()

In [15]: if queryset:
    ...:     print 'yes'
    ...:     
(0.001) SELECT `gov_host_info`.`id`, `gov_host_info`.`created`, `gov_host_info`.`modified`, `gov_host_info`.`name`, `gov_host_info`.`host_url`, `gov_host_info`.`purchase_path`, `gov_host_info`.`is_open`, `gov_host_info`.`gov_level`, `gov_host_info`.`memo` FROM `gov_host_info`; args=()
yes

In [16]: if queryset:  # 第一次之后则使用cache
    ...:     print 'yes'
    ...:     
yes

```
4. 使用exists来判断存在
```
In [17]: queryset = GovHostInfo.objects.all()

In [18]: if queryset.exists():
    ...:     print 'yes'
    ...:     
(0.000) SELECT (1) AS `a` FROM `gov_host_info` LIMIT 1; args=()
yes

```

5. 使用iterator避免cache
```
In [19]: queryset = GovHostInfo.objects.all()

In [20]: for g in queryset.iterator():
    ...:     print g
    ...:     
(0.000) SELECT `gov_host_info`.`id`, `gov_host_info`.`created`, `gov_host_info`.`modified`, `gov_host_info`.`name`, `gov_host_info`.`host_url`, `gov_host_info`.`purchase_path`, `gov_host_info`.`is_open`, `gov_host_info`.`gov_level`, `gov_host_info`.`memo` FROM `gov_host_info`; args=()
5-舒城县人民政府网
6-霍山县人民政府网
7-寿县人民政府网
8-霍邱县人民政府网
9-金寨县人民政府网
10-金安区人民政府网
11-裕安区人民政府网
12-金安区政府采购网

In [21]: for g in queryset.iterator():
    ...:     print g
    ...:     
(0.001) SELECT `gov_host_info`.`id`, `gov_host_info`.`created`, `gov_host_info`.`modified`, `gov_host_info`.`name`, `gov_host_info`.`host_url`, `gov_host_info`.`purchase_path`, `gov_host_info`.`is_open`, `gov_host_info`.`gov_level`, `gov_host_info`.`memo` FROM `gov_host_info`; args=()
5-舒城县人民政府网
6-霍山县人民政府网
7-寿县人民政府网
8-霍邱县人民政府网
9-金寨县人民政府网
10-金安区人民政府网
11-裕安区人民政府网
12-金安区政府采购网

```
