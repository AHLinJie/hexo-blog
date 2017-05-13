---
layout: post
title: excel处理
date: 2016-07-22
category: python
tag: excel lib
---
**摘要:**
工作中有需要使用excel的场合,python中有很多excel的处理包,这里使用的是xlrd
记录下一些常用的方法,方便以后查阅.

#### 依赖包

sudo pip install xlrd

sudo pip install xlwt

sudo pip install xlsxwriter


#### get csv picture and save it to excel

```
# coding=utf-8
import os
import sys
import xlsxwriter
import xlrd
import xlwt
import urllib2
import csv
import time

reload(sys)
sys.setdefaultencoding('utf8')  # 中文问题处理

"""
## 处理csv 文件中图片链接显示问题（生成excel文件，保存图片）

- 打开指定的csv 文件
- 读取一行　记录　
- 获取　记录列中地址图片　并保存
- 将记录和　图片组装　添加到excel 中
- 删除本地保存的图片
"""
def download_img(file_dir, img_url, img_name=None):
    if os.path.exists(file_dir):
        abs_dir = os.path.abspath(file_dir)
    else:
        os.makedirs(r'%s/%s' % (os.getcwd(), file_dir))
        abs_dir = os.path.abspath(file_dir)
    img_name = "%s/%s" % (abs_dir, img_name)
    if img_url.startswith('//'):
        img_url = 'http:' + img_url
    try:
        data = urllib2.urlopen(img_url, timeout=3).read()
        file = open(img_name, "wb")
        file.write(data)
        file.close()
    except IOError as e:
        print e.message
        return '/home/jishu_linjie/Desktop/tmp/unget.png'
    except ValueError as e:
        print e.message
        return '/home/jishu_linjie/Desktop/tmp/unget.png'
    return os.path.abspath(file.name)  # 返回绝对路径


def write_to_excel(file_name, collect_data):
    workbook = xlsxwriter.Workbook(file_name)
    worksheet = workbook.add_worksheet()
    count = 0
    for item in collect_data:
        count += 1
        index_s = str(count)
        if item[0]:
            worksheet.insert_image('A' + index_s,  # 指定单元格
                                   item[0],  # 图片地址
                                   {'x_offset': 1,  # x_offset 设置偏移量
                                    'y_offset': 1,
                                    'x_scale': 0.2,  # x_scale  设置图片缩放
                                    'y_scale': 0.2})
        else:
            worksheet.write('A' + index_s, 'img')
        worksheet.write('B' + index_s, item[1])
        worksheet.write('C' + index_s, item[2])
        worksheet.write('D' + index_s, item[3])  # 图片链接
        worksheet.write('E' + index_s, item[4])  # 产品名称
        worksheet.write('F' + index_s, item[5])
    worksheet.set_column('A:A', 17)  # 设置列宽
    # 设置行高

    for i in xrange(count):
        if i == 0:
            continue
        worksheet.set_row(i, 125)
    workbook.close()


def main(fname):
    print(u"You file name is :%s" % fname)
    reader = csv.reader(file(fname, 'rb'))
    collect_data = []
    try:
        count = 0
        while True:
            item = reader.next()  # 使用迭代器输出
            # item= ['822292480043',
            #  '15372',
            #  'http://www.hh.com/MG_1466576587583.jpg',
            #  '\xe6\xb0\xb4\xe5\xa2\xa8\xe5\x8d\xb0\xe8\x8a\xb1\xe8\x88\x92\xe9\x80\x82\xe4\xb8\x89\xe4\xbb\xb6\xe5\xa5\x97/\xe7\x99\xbd\xe8\x89\xb2',
        
            img_name = ''
            if count != 0:
                img_url = item[2]
                img_name = '_'.join([str(fname).split('.')[0], str(count)]) + '.jpg'
                file_dir = str(fname).split('.')[0] + '_imgs'
                img_name = download_img(file_dir, img_url, img_name=img_name)
            count += 1
            collect_data.append([img_name,
                                 item[0],
                                 item[1],
                                 item[2],
                                 item[3],
                                 item[4]])
    except StopIteration:
        print(u"文件读取完毕!")
        write_to_excel(str(fname).split('.')[0] + '.xlsx', collect_data)


if __name__ == '__main__':
    fname = sys.argv[1]
    if not os.path.isfile(fname):
        print (u'文件没有找到！')
        if True:
            time.sleep(1)  # 程序暂停1秒　这里没有意义　写着玩
        sys.exit()
    else:
        main(fname)

```

