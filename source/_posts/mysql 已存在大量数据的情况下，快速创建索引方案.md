---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46627UDHBGU%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T160049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEE%2FIIY9gDHoxGUNTgROBvu4BdKZEeXcYq%2B%2FyPVLPW9VAiATCQsD9PjBTk%2F2VbO1O687byfJLnsA%2FcaSjYY9L6BShyr%2FAwgtEAAaDDYzNzQyMzE4MzgwNSIMYo6uPfeYjrQHMMs7KtwDE%2BIN4516J3tSdr7q3nqKmhyykNiLplufmTapmi7sTgr6l8MWE0iJ4%2Bt1nofaaGTI8m9YojnwUX%2BwkE2ut2NrwEEYsvLjWSY0woqhXdWe%2FWXFFTVIMq1Xlg9PbuXN16AUkVGNB9%2Bclvz%2F7J9defYvp4pF%2FA4AUqELiNefckQcotUv27cSESbpZKddEFQZdqq5kL2kYr%2BC6PIOLpgl6Gl1YYLRd54FMh0lkxk%2BHlJ42POFZ6o8tm7%2FuKvEabuhi%2F2DT%2BkIuXdg%2FdLpKxpQ4ZwxMLVUb4DLqol4XohQwkM56QC3ShTtKP1JfuwGAU6kI9BznIB6QJrZT5dF6Y%2BP9mqBWMc67Zl%2FOpjq%2FMTqjduMHdEmZmWQVUsjY63VeT2DwmyscGQjgwm%2FXcldl206ICLMmTmQ07uHGpQCZdXYqMA7yqvdFG4Hzs2X%2FCPwJ68ddiFA3PoDB0z1B0w7Ectljirz7FQMo%2BzlqW7VcTD0OdjguT0ijwgQ1P%2FDhFLXbgeBqMcA%2BD1uEQgrf8OBetYRRummVMczGpdmGshJBzPQMTBQcHy1V0dBcrM%2FqkppbjY1DDe3jOfjrq%2Bhgio1HTY0p4PTPJrHrvytf37yMnA1LshVqJZaB%2F0p1XVMEYbsz74w%2BLeuxwY6pgHRnDIOMbrClIhcefHjX9jlbg2B5kvBNWwfPGpQnA11JOVwN7n3PAZrEGD%2FP31a9q8BnnCdjZFG1R1v5yIqk3kD5uZmXrYwn%2B5tDlVzd0egRepYCJW%2F70ADabUZms8%2BLAERGxfNS3IlDW8B3qtLd1mHQsRPX7AOEGqrt4AVKyPfm%2BFmzxELS%2FeiG5hg%2Brqf7u5gRDNugfWZf5yf3ajBUAUP8Snl9Bpk&X-Amz-Signature=9b4d540007db3705a1bbcc64a98cd08b56794cb7fb90fa0388039c4fe254c155&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 13:56:00'
index_img: /images/c34f92fd7edfbc072452166489949590.png
banner_img: /images/c34f92fd7edfbc072452166489949590.png
---

# 原因


最开始设计表的时候，没有设计好索引，等数据量多了，查询效率慢了，想再次简历索引。但 数据量巨大，一创建索引，数据库直接崩溃


# 解决一（慢 但是不影响系统使用

1. 设置mysql文件导出权限
2. 导出文件为txt文件
3. 创建一张临时表，与原来的表结构一样 `create table text_assets like network_assets_blend`
4. 导入数据到临时表
5. 导入完成之后，将原来的表改为其他表名，作为备份，将原来的临时表改为真正的表名。

# 解决二（速度快，但是影响系统使用

1. 直接备份数据，导出sql文件，（这一步几分钟
2. 截断表（就是清空数据保留结构
3. 建立索引
4. **将sql文件中的删除表结构和新建表结构语句进行删除（重要）**
5. 导入sql备份文件

# 解决三（保守一点


就是方案2的改版，额外创建出一个临时表来存储数据。

