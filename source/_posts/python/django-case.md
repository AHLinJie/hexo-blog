---
layout: post
title: Django Case
date: 2016-04-13
category: python
tag: django
---

**摘要:**

在项目开发中使用的是django, django除了为别人所诟病的"重"之外还是有很多的好东西的
比如说orm,比如说admin,比如说middleware,比如说cache等等,开发效率很快,分分钟就可以
做一个后台管理.其官网的更新速度可以说是快得令人发指,记得我们的项目开始使用的是
1.4.11, 15年出来了1.7,后面相继出来了1.8 1.9 现在项目都已经更新到1.10了.



#### django 使用过程中的小case

##### Django template tuple

- use choices in template

```
class P(models.Model):
    A1 = 1
    A2 = 2
    A3 = 3
    CHOICES = ((A1,u'des1'), (A2,u'des2'), (A3,u'des3'))
```

```
render_to_response({"choices" : P.CHOICES})
```

```
    <div class="col-xs-2">
        <select name="ware_by" id="ware_by">
            {   % for  cid , show in choices %   }
               <option value="{   { cid }   }">{  { show }  }</option>
            {    % endfor  %   }
        </select>
    </div>
```

---


##### django debug

- space not enough
    ubuntu 14.04
    python 2.7.6
    django 1.8.4
    ```
    [2015-10-02 04:10:46,441 pyinotify ERROR] add_watch: cannot watch /usr/local/lib/python2.7/dist-packages/django/contrib/admin/locale/io/LC_MESSAGES/django.mo WD=-1, Errno=No space left on device (ENOSPC)
    ```
    [Refrence Link 1](http://stackoverflow.com/questions/27948612/django-test-run-environment-error-no-enough-space-left-on-disk)
    [Refrence Link 2](http://blog.csdn.net/myarrow/article/details/7096460)
    [Refrence Link 3](http://www.jincon.com/archives/119/)
    run cmd in ubuntu terminal： `sudo sysctl fs.inotify.max_user_watches=16384`  
    
- system encoding error  
    ubuntu 14.04  
    python 2.7.6  
    django 1.8.4  
```
    UnicodeEncodeError: 'ascii' codec can't encode characters in position ...
```
    [Refrence Link](http://blog.csdn.net/sky_qing/article/details/9251735)
    
```
    reload(sys)   
    sys.setdefaultencoding('utf8')  
```


---


##### Add A Logger To Print Log Info In Console

```
LOGGING = {
    'version': 1,
    'disable_existing_loggers': False,
    'handlers': {
        'console':{
            'level':'DEBUG',
            'class':'logging.StreamHandler',
        },
    },
    'loggers': {
        'django.db.backends': {
            'handlers': ['console'],
            'propagate': True,
            'level':'DEBUG',
        },
    }
}
```

---

##### Logger 
```
import logging
logger = logging.getLogger(__name__)
logger.warn()
logger.error()
```



##### Get The Same 'name' Form Data In Django Seriver

- form 

```
<form action="" method="POST">  
<input type="checkbox" name="abc" value="123">  
<input type="checkbox" name="abc" value="456">  
<input type="checkbox" name="abc" value="789">  
<input type="checkbox" name="abc" value="110">  
</form>
```

- python code


```
a = request.POST.getlist('abc')
In[i]: type(a)
Out[i+1]: list
```

---

##### Http Request Save Many Value
    
```
    for k, v in request.data.iteritems():
        k = str(k)
        if not hasattr(instance, k): continue
        instance.__setattr__(k, v)
        instance.save()            
```

```
    if hasattr(instance,k) and getattr(instance,k) != v:
        setattr(instance, k, v)
```


---

##### Add Data Into ManyToManyField

```
my_obj.categories.add(fragmentCategory.objects.get(id=1))
my_obj.categories.create(name='val1')
```


---

##### Django ORM: only

```
In [29]: ss = shops.only("shoptime")
In [30]: print ss.query
SELECT `table_name`.`id`, `flashsale_tongji_shopping`.`shoptime` FROM `table_name`
In [31]: 
```
---

##### Django ORM: only

```
shops = ModelsName.objects.filter(linkid=5).only("shoptime").dates("shoptime","day",order='DESC')
In [19]: shops
Out[19]: [datetime.datetime(2015, 11, 27, 0, 0), datetime.datetime(2015, 11, 24, 0, 0),
datetime.datetime(2015, 11, 4, 0, 0)]
```
**dates:de-weight the record**

---

##### Django ORM: annotate

- if not use 'values' there will be `group_by` all fields

```
In [12]: clks = ModelName.objects.all()

In [13]: a = clks.annotate(sum_clk = Sum('valid_num')).filter(id=5)

In [14]: print a.query
SELECT ... all fields ...
SUM(`table_name`.`valid_num`) AS `sum_clk` FROM `table_name`
WHERE `table_name`.`id` = 5  GROUP BY ... all fields ... ORDER BY `table_name`.`date` DESC
```

- first `filter` then `annotate`

```
In [17]: clks = ClickCount.objects.all()
In [18]: a = clks.filter(linkid=5).annotate(sum_clk = Sum('valid_num'))
In [19]: print a.query
SELECT ... all fields ...
SUM(`table_name`.`valid_num`) AS `sum_clk` FROM `table_name`
WHERE `table_name`.`id` = 5  GROUP BY ... all fields ... ORDER BY `table_name`.`date` DESC
```
**so there are no diffrence 'filter' order about 'annotate'(django 1.4.11)**

- use values filter `group by` field

```
In [15]: b = clks.values('id').annotate(sum_clk=Sum("valid_num"))

In [16]: print b.query
SELECT `table_name`.`id`, SUM(`table_name`.`valid_num`) AS `sum_clk` FROM 
`table_name` GROUP BY `table_name`.`id` ORDER BY `table_name`.`date` DESC

In [17]: 

```

---

##### Django ORM: filter range

```
In [22]: tf =  datetime.date(2015, 8, 21)
In [23]: to = datetime.date(2015, 10, 22)
In [24]: xxx = ModelsName.objects.filter(created__range=(tf,to))
In [26]: xxx.values('created')
Out[26]: [{'created': datetime.date(2015, 8, 21)}, {'created': datetime.date(2015, 8, 21)},
{'created': datetime.date(2015, 10, 22)}, {'created': datetime.date(2015, 10, 22)}]
```
**range filter contains the boundaries**

`created__range=(tf,to)` could replace `created__range=[tf,to]`  


---

##### Django ORM: extra
- select
xx = queryset.extra(select={'alias_name':'model_field REGEXP "^abc"'}) 
```
SELECT (model_field REGEXP "abc") AS `alias_name` FROM `table_name` 
```

- where
ss = queryset.extra(where={'not model_field REGEXP "^abc"'})
```
SELECT * FROM <table_name> WHERE NOT <field_name> REGEXP '^abc';
```

**add and senctenc in extra**
```
sss = ModelName.objects.extra(select={'created_d':'date(created)'}).values('created_d','customer').annotate(sum_value=Sum('value')) 
```
*convert the datetime to date then group by*

```
SELECT (date(created)) AS `created_d`, `table_name`.`customer`, SUM(`table_name`.`value`) AS `sum_value` FROM 
    `table_name` GROUP BY `table_name`.`customer`, (date(created)) ORDER BY NULL
```

- extra and order by
```
queryset = queryset.extra(select={'refund_rate': 'total_refund_num/total_sale_num'}).order_by(
                'refund_rate')
```

---

##### Regex

```
for modelid in modelids:
    x = r'(,|^)\s*' + str(modelid) + r'\s*(,|$)'
    descriptions.extend(
        ModelName.objects.filter(detail_modelids__regex=x).values('id',
                                               'detail_modelids',
                                               'description'))
```

---

##### Same Field Filter

```
from django.db.models import F                     
ModelX.objects.filter(a=F('b'))

SELECT * FROM `table` WHERE `table`.`a` = (`table`.`b`)
```


---

##### Django Modify Model Field Step

1. modify the model fields
2. python manage.py makemigrations <app>
3. auto gen the migrations files in migrations director(do not modify the auto increce num,but can change the name)
4. python manage.py migrate <app>

exception handler comands:

python manage.py migrate <app> --list  # show the migrations fils exced list

python manage.py migrate <app> <0003> --fake  # fake the 0003 file was excuted, if you want to skip a migrate file


---

##### Django Dumpdata Command

```
python manage.py dumpdata coupon.UserCoupon --pks=16,17,18,19,20,21,22,23,24,25,26,27,28 --indent=2 --format=json > flashsale/coupon/fixtures/test.flashsale.coupon.usercoupon.json 
```


---

##### Django Limits Of Authority

[Reference Link](http://www.tuicool.com/articles/RB3yAb)



---

##### Django Setting Celery Work

`BROKER_URL = 'amqp://linjie:123123@127.0.0.1:5672/vhost1'`
`python manage.py celery worker -l info` 


---

##### Django Transaction

```
@transaction.commit_on_success
@csrf_exempt
def mama_Verify_Action(request):
    mama_id = request.GET.get('id')
```

[Referance Link](http://blog.sina.com.cn/s/blog_3fe961ae010167ah.html)



---

##### Django Admin: search_fields

django admin search_fields()  # search str in database where search input content
1. search_fields=['first_name', 'last_name'] 
1.1 searches for“ john lennon” 
WHERE (first_name ILIKE '%john%' OR last_name ILIKE '%john%')
AND (first_name ILIKE '%lennon%' OR last_name ILIKE '%lennon%')
2. search_fields=['^first_name', '^last_name']
2.1
WHERE (first_name ILIKE 'john%' OR last_name ILIKE 'john%')
AND (first_name ILIKE 'lennon%' OR last_name ILIKE 'lennon%')
3. search_fields=['=first_name', '=last_name']
WHERE (first_name ILIKE 'john' OR last_name ILIKE 'john')
AND (first_name ILIKE 'lennon' OR last_name ILIKE 'lennon')


---

##### Get Ip Address

```
def get_client_ip(request):
try:
    real_ip = request.META['HTTP_X_FORWARDED_FOR']
    regip = real_ip.split(",")[0]
except:
    try:
        regip = request.META['REMOTE_ADDR']
    except:
        regip = ""
return regip
```

---

##### Django RestFrameWork Request Update

```
query dict update : 
request.data.update({"turns_num": turns_num}) 
# update directly in create method
```

```
request.data._mutable=True 
#  use _mutable to open mutable the request.data in update method
request.data.update({"a": 123})
request.data._mutable=False # close mutable
```


---

##### auto_now datetime 
```
modified = models.DateTimeField(auto_now=True, db_index=True, verbose_name=u'修改日期')
instance.save()	# that will be change the modified field
instance.save(update_fields=['field']) # does not change the modified 
```


---

##### Django QueryDict to Dict

```
QueryDict.dict()
# fro example in django restframework
request.data.dict()
```



---

##### django cache 

```

from django.views.decorators.cache import cache_page
from django.http import HttpResponse
import json


@cache_page(10, key_prefix="test")
def test_cache(request):
    print ("no cache")
    return HttpResponse(json.dumps({"device": 'iphone'}))


def ttt():
    from django.core.cache import cache

    cache.set('my_key', 'hello, world!', 30)  # 缓存key value 缓存时间
    cache.get('my_key')
    cache.add('another_key', 'new value', 30)  # 如果是已经添加过的则返回False 否则添加到缓存并返回True
    cache.get_many(['another_key', 'my_key'])  # 获取多个key
    cache.set_many({'a': 1, 'b': 2, 'c': 3})   # 设置多个key
    cache.delete('a')  # 删除缓存
    cache.delete_many(['a', 'b', 'c'])  # 删除多个缓存
    cache.clear()  # 清楚所有缓存  注意使用

    cache.set('my_key', 'hello world!', version=2)  # 设置版本
    cache.get('my_key')
    cache.get('my_key', version=2)  # 获取对应版本的缓存
    cache.incr_version('my_key', version=2)  # 如果设置了 version的需要指定version升级 否则异常

```
[文档地址](https://docs.djangoproject.com/en/1.10/topics/cache/)

