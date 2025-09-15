---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666LBA3QYN%2F20250915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250915T183042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAEaCXVzLXdlc3QtMiJGMEQCIHOeh7XpkTxnoOGyKdf%2FfXgkFYK%2FdNrXl3oI31eYrXsfAiA0QrgXJRqpTBQE5J4HROSNj9UOGyieUloRoVI09KNyLCr%2FAwh6EAAaDDYzNzQyMzE4MzgwNSIMrkFAoH40cv%2Bpy9VTKtwDX%2F0ffF%2FA79zZ75pSe7hvCOH6iGXD4Ra89RjrjXC%2FEQmzcsEjAVRMNuEa4F9EUkCNnMSVJFAx9S42Yk1FMKmjEfKnaab0139Ak%2FQR%2F5BdIW822fUBTYfoDTxqs77RemT7MElJpKw%2BStRrBRDozjEJ97guZxk5hO%2F3uohiNjPVbgqhwkCIJt7mYGIizOcyFPQSAZPX%2FP8eZX40NZYr0C09pRQ8V90SKUjvWJwtbWgtwt9GP2GZbwwGGPf0lf6jaDxbb1jO8xAP3On%2BQjWGPL3gLP7mdfWgaRY0ljDyAFQ%2FRTEEd2yXqO7E7TTdaN5cFSfIUpIY228JfBKdnjRkw0MeTPyZ3jiw7A6XOLoCPFoaDHmiuLrLNyYaoH4KfPtoDd0L7U9KDly%2F8abjpB%2BaJRSXhnYEz%2By%2FklCJi3cbCsxJ8fIV5aSGWi5P8kQv0MZ7hITYWGd1%2B3qMR%2BOcX%2Fb6XARBGGpcC4b%2BFT7h%2Ftf6ggXPJ%2F9OfMLYksQkwbWbdo8BCnvKJCBQJ1cpr6OabyUx2yElNFDvhPdj%2FI1OjKZUP4cRepZkWansO5ORK5gfB83CmK9Qtbzz5MJWDmbQXnOEs4usTTtVRtE5M76Cp1%2B523kpOE7Qw4q18yMHsoA7SEIwr4qhxgY6pgFh1oROvIznyl5360xBdwiC00mzwqHMwsrFZ4s2WMrgkl5XItkyuVqxE82WVzcE1Ob21PKpZyZ%2FrADQzUtxpjyYMQWPiMjtDV%2FpVm%2FA%2BJJfev9o7uwzcO6yxU23sc%2F7lkS91esMZwuupqrtJ8dCA9Ww6AM4BP0Z9mkss4ffjCZjF4wrERrhd7H3ozzPGRnE7Os5uqyFZMBzjKQVs8tcOCfqmqxuT9U%2B&X-Amz-Signature=d7cb506f0c8a86f9007eea090c7d9c453fba9a33a321fc44a7fecfe34152f1cd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:56:00'
index_img: /images/c34f92fd7edfbc072452166489949590.png
banner_img: /images/c34f92fd7edfbc072452166489949590.png
---

# 原因


最开始设计表的时候，没有设计好索引，等数据量多了，查询效率慢了，想再次简历索引。但 数据量巨大，一创建索引，数据库直接崩溃


# 解决一（慢 但是不影响系统使用

1. 设置mysql文件导出权限
2. 导出文件为txt文件
3. 创建一张临时表，与原来的表结构一样 `<font style="color:rgb(77, 77, 77);">create table text_assets like network_assets_blend</font>`
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

