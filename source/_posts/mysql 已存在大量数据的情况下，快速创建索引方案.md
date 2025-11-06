---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UN2KFNS2%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T010039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIB7D1GQP0FNLwy5Soy6Wg0g2l8PGw2DQ3nAgTtsEtLu6AiA1yoRtWNBP7Nr9HTfgVLoV6bprKsk4Z6KjOi9UXH5dlSqIBAia%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM2Pjn3AThvuQVYglzKtwDjVJbaMEThicEwG3H10TXbetT5y1ksjTaTUQQIYobQMWxEM210XBShHxsKCLiDWDHyibI7JVb28Sti4Z6FGHMug2UPFbFXB5z%2Fekp9sXHhgP9WLKAc8dHAhpp9ofaez%2BvG3DH9XAfyYKc0qiSyEry6wal3cqt%2BRUkNusDmvIVOZQKtapyi5TqgjKtIelMFCVWr2wML71R1AJO8Cez7K%2FMZm1GW2s7AVKU5sDDqJK3G5gbPS2ZJD2GfANu5WyULo%2BeKUqejgJZG%2F65VghnCVYsnDNbbKUhF4b9ulp8L3ViFtSsKt7zfh71ykjEmpi%2B3o0bRSSq619veb7Or6UciXOkIwC391Na%2BiUd52kodwU5ixdSC0%2FM8SidgUuK4jJjySbcxq3rKKX3ah83ut%2BXqECQHg1RhYbnWXTXuLbJO8hCVac8nVp5yJZqG86jHGi2QogfjiNaN1tv7BhprcIjUce6UAiGaEQTUUPI%2F6gxvkVEJM%2FVvjuLd5g5ZW5PIbA60AVfvono4IUB24gB42l0lwa7QPjotVovCAZ1XjdAD%2Bhk7sGmLC%2FthwSl5jrHjb0AC1Sr2Q3Fu2ymPJ97E9tX6Ey7T7W2mMxrnUFEYvR7iI8BduoVyhgdDMpQddcPdUIwstavyAY6pgHjhXiJfrBW9jPtem0dpcVJBgJLDiOGW%2FBnJLvyMZMf4rGtsv2y84bIyRIbNTZGpfNcpoik2XbW3TyB8svPJZ3mjrMGFTlYt2KAa7aY%2Bit%2FwPQVReVlLpTr6JelTdXFBCqqyjhuFiEeQCmx4SWVOPZrP8dYDvZPJ53dJbDjKQiLjilMIALzKTn57P1fOw9KFMvfmcgIamT4TY%2B5S9BtjdfiaNEhqvY7&X-Amz-Signature=ba1584133fe3c337a806e7d68be92008cb6a29f9ccf6dc0c879469d744626445&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

