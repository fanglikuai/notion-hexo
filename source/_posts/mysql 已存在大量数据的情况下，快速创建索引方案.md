---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VCZCBOAC%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T030044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDKLWd%2FXpfOgmOT2hxKeAQg7R2IJ%2BcjMU1ZAzuQ4eRvGgIhAJlD9B6JXCcH2GkRl3vFgVvCVLOvpypNDguoKGaZJTX7Kv8DCGMQABoMNjM3NDIzMTgzODA1IgzEFLQs9dVO2tPtUU4q3APE%2F4hhz4szsxmsH5KIdZAktmL79mvq1NUbOpBt9ZOFQHhRAAeOmKrwzsD9WG9RwFu2DCmbn6CZZ5nUuaCf8v4aKQL90JDrtG9%2Fq0Kz%2FSTsrNkH4Yb1f7JebDIgy2Dqknku77hRdFvP3%2FMnF%2BlmpC0HTME0kyF4kWPzTmw%2BHQDVrbwMI6NZJFN9%2FetaLMU5wr8W2x%2BbtEYyUse8tIfEJ7W%2F89TjT8jkrv7KrOW88PYwrWwufec1MKK5rLkvCNNe7KJAEdSIJ1bAlFyMqtWowvbSGy8MFAlZvRTJhkffvNLkVbczozEwcVlMRSHKV4M6VzLicoejehZS7Kx7%2F24hCEhwMage4esM%2BeOG6xh18tSW9wTsFUgtbAZs2wP%2BSx3vZ0IZfXJZRp9Gt5z5N6qrl6XbQseOgkhm2rLk9O2o0G04s1u3rdWUpJtzPilE6aRYvE6qd6BcMAtNW%2F3PGtF5HXm1tSmymX3q1u6m4%2F6AnkClsGHljH3OVg%2Fz6rsimavcXNEZyY9uEFqaEpSTjSeLuJy%2B3AoG7OvxrOP3jT56xDu3601y3%2BsoYCxj6MqwkT0uyH3y7Zu62vT1ejNmmnIVYpWrwynApQcgNnakke89YdbdI2AIiVMIF7jktQVtOzDwmJTJBjqkAaPVhz%2FIH9j5neDtFBNG%2BS3WrJ%2BFdhYBKTxcLHae4jA93T75sVd7ht6ZAyMAHS5XC%2FNuXe%2Fm58YU0RM3FDP9Fc8eb1L0RNW7XdJnwQ9JcaiGrbU8w4QtLG7in1MofoXKgEkITX5EH91M%2FyUSicVLMUXLt3EvyX7WyegCdDazS%2BOkgSJRjtt4lRbQnfig4Is75R2VQ7NqWe4HRPQNJxx%2FGrJLM7ms&X-Amz-Signature=636c4cf47e1cdef75f31c5f220c8dab64fab7057e46e6093615ca7eb2c023fed&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

