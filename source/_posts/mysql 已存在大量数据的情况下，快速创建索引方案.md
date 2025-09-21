---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665A3ETUC5%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T190044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCID5kekUBd97qhKo%2BoGj7l4JKrzBgmPuQbEgQjRnuQ8MGAiA90C2Z1D8i4MBQOb%2BuKOC6snN24BKV3OsMo7f3qtp9Zyr%2FAwgbEAAaDDYzNzQyMzE4MzgwNSIMaaijw2e19L2hb172KtwDUbGqGZDtWUx%2FqMdzGBrK2lqgwMSymlJlOQ0M9xZTQLrLds177MHUAGD7CrMNXRvrsrMbD3MATGWXMzWQNr8K1I9NOdzimIaqSLKkzX8t1XSoS4U%2BOso6%2BZEhZ7BuKZybVWfz8m8gRao9HdtC6ztBYWyL%2Ffugul3GK9TpIWaAKJAWdYVkoUrykYiF7%2BTuEs0Lu8kIz1X%2F85ZKwP5Jd3OJqVhIMovb6r965E9UupSrTgPPyp8EL9Doo8KafgWu1n2iDzn9ZFkP3fYWw3AtFKwJbDMd3QI0Wgn9lP5Sr0VZhbkC5i3RM0eCWh6WTfliwKbZ2SRTwBUkNHpCVhka6f5vdqiVVuR7gL%2ButNmM9JMtvyLJ19fpKI12cmeBX9A2xQh300BT101jGCXarGIggkUsI%2FMKh2otd%2BRYwKxCIljzae3Y1K5NZkHMr5AcULGdV%2FonwOtZCf4YMSVKf4QICue%2B4Elll%2FKJ8tHieXM1P2FopZ7rDUagLmyUSY7wfZbdSLz9zJyiMkN9%2FqoZz6Ef6hBkI0tAqDJ0JDGhvn1InnGwUeKETvLsfYRNIjFliOrJo1i1oYerBoYqQAKIXPa%2F9hU894YxxnZcQlWhDivQax%2FwAuSDKaFYVu3zXQb0cWQw3OnAxgY6pgFcJN2%2FOPK7zd6r9bSiDW%2F0nRiGWzhKPxq%2FePU1nbber%2FQ5372Wg9fEVdoWK%2BjxQa9VzQ%2Ffv5Z9bN%2BHcBlenMGjpGdrUKh1Shsszit4P0Lhh3Nb1o7AK%2BDAy3MMwknb71b6x2%2FtK7GEjnHZWKnozRD6sioOCtvRfJFCGgZBo99mvzBjnuOE1Ocr4FpNGScZ45TGyfWrT8imepYgRJu%2FrSSWGEZ9qmZn&X-Amz-Signature=1315c84ee10236001056952073d1e4be5ac1e873f4c67cf8e4372662b17dd9b8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

