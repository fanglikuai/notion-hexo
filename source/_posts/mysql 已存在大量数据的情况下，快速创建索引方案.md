---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q5UTWZDI%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T150103Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDYaCXVzLXdlc3QtMiJIMEYCIQDPPvryBc3EKriKTBIJxVX4CzHXnG0eNBzwK2bFT6ZTDgIhALZ49kDWVVubzUl1aRckwKhahnwef5EQppj3ZBrRKxQ3KogECO%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyuUr25SzAj%2FKsq%2BV4q3AMEHaxt6C26VkGQOg%2BmgKcRv%2FFE5LoSHU1MgaH0rqyqdcDjwKZit6nCnLGuH223Jz4XUTFRlYq5c2ER2Qao%2BXNHQhr09FFmzs1EjL19NQ6GAEc68URFUAPLbyvwZriPtkWMfKpF1rDpMKqtMtY3sRFO8%2BzWxTVk7TcT85OA4pEbBkBaCifWs3srWjvc8%2F3CYNJz46tq%2B96gHfF052KboU4t4KvNn%2FCG1zx0jtQO78x5Tgle4n4nDovzkKkVIjrKW3MzyLADfKZhfnuBFKDjK2hUKhHzjkqlrqKdqgEYbkdA3YhGJIDrYbSrFcbcgWS4P73o%2BsCLxuU9H46rxhvdZxfS815Knv33wqb0%2BylwY1FQlAvjWkubB58cu%2F5p6Nv%2BqMLX5nwX%2FWtrioMpbYBc9rOh%2FH6u4axD25suQ7qFUTIZYxPNuRfTId00tUXZ55MQ2nq2pwm11s0aluxf%2B%2Ffcm38tSAUsfbQFCaA7Cc6QNxsGeTsRxOb2j5T0XGAywb8lRoDllIU28%2BYW9Y5QQS4SK9DU5u6Z78f0J8U4vRn1pNhTVJ0WtadZiFjkLRmi47IBo1MD%2F2ZRd366zRXB8D%2B0lmILAaqI%2FBB7qWlL3k%2FeiJ23l2CmF%2FMgvHpMTFANKzDf2Y3IBjqkAdOIZorkZdte2CzK6iTDr75yEFnVKi%2Bcd6g1lWm3Xtrf7bY1KnCypHJj9V6KgDrmYo7UhziVfdA9c0nK%2BXfwVToGTk2uVIPlnVXtINgEkjYcHyIhSZpWBRowqA9cT2XjroApnoPixFngyz7o0bUcB52UU17s2t8sSt%2BXUTBVVI2YIoRZc2eHdD%2FSxNVl2x%2Bq3Zv1PZrBuL6ZbsKAzmMhO3svO9xw&X-Amz-Signature=9636a8066ba9d578b0a12755f60462f0ec1ea6d5d251eaa8d8fb4dc3f4857555&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

