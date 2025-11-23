---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QWY3V3DD%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T100050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJIMEYCIQCRFXds8GfZS9MmL9utClFxYWTDqJW1DoH4m0dnB3pc1AIhALVWLs5qmrfzVvGYkvXzyxgqDseXWf2raTl6kc76DZTIKv8DCDoQABoMNjM3NDIzMTgzODA1IgyNQBTg9lk9oFCmXboq3AMeAqqUlUVWbms29J3ww8OYvNsQoD%2BsDyfHNcoEY4%2BGmggGZbJWanqaBO10wmQcP5Xs0HNf4RGBlPORsmNjG4teBTwLgj6UX9uMi7sZG6WZE%2Fx7dPNN8C3QvpaIJaQeklURxtd8rC7ICitGX8O2fxzgfOFKN%2BomCLkqYU86hIwJ2OdjbkKd9FpjJnHJJ3XLJNXALTzzc%2B3O4cm2xbkZ%2BzfydPftkZTmNPpevHEA2hottQ6aRb7k4unr8YwHz2bfh%2BFfJRi3hHKApiz6%2BqG%2FvCJ3Wa%2BpJhXOJz4rlhY01e02OaUWn3Kc4B5OtNZKgm02doqc7qw4PasEJpWsfs7iWDdFtDzAROCEhyRSR0jI5BpP3hneVeMkEbE8mRG2f313Qk80SSFiehwLtmG1witgbfhvi5MTL42m%2Fko1%2FCR3zIuTClbaDMB6BuVfFsHS1jljvfTbzI5l2h%2BNtPF%2BC%2Fr53Cd3StP%2Bz2WN%2Fi2Dwc6KDQHETArnUqSgl7x5J56meGoc%2FnpZdmje5v1P98MYzn5Xl6LgfWDcIFwPWa3f%2BeBoYk82PAnPXXFovaSrQRtDI%2BUK7tPehAahSlUwB8t7oKcQPj5fXvYS7waoUOngA%2BhOuvXw080p8QqxtIugrIq96DDol4vJBjqkAWoDBB0pAN%2FdIRnCCfFJAVxoMKwwbj15plngBYhY2%2F1nwunxiCsAlq49PJH5JxihGabPhp73rBEG1bAy5qTl7P0Ut3GYXCyT4nJE1qVX3F3OA4NJpzJGtwAF6nGp1kzkVVAdJq3Yv1XDn62MholiPhV75UIrKVAt13wEpqkODtsBNRtUz0P%2FQEO7triECE9v1fI09NHnuS0cwYc8dmz7MuXrtTOy&X-Amz-Signature=e66c0851c5a2959696c7b11597ca0c296ac572cebd2b9b858776177938a600ee&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

