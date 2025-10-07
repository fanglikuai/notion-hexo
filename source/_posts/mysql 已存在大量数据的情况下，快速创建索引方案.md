---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZLJRR6DV%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T120051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAwaCXVzLXdlc3QtMiJIMEYCIQD7pYjByHqyIan9ZkQHDLEvcLmfhjhMw5B8EbAbK%2B5wYgIhAIVZgd6ZmTW%2BX7ZWHGq%2Fc8ndfRw5HmwXmZnn952l1m7OKogECKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzXPPktjyoa%2B4%2FFBzMq3APO5Mg8fu4WCxMfdBuc61RHHhToi1%2BS8nT91tHVlLEkwQ9b1sa1Fr%2BtF3VOM4R3OqDAP4CDQD9NMLzaeVYsTIn7U5uEGafvMKUNJjx33%2B6qiwwrSmQQA0WE3U%2FKzPQW2O%2BuTSMC3VwjD8DDLBjy4ZTU15y0soWQ8LQ5wCn9XSvNfUHH9tR%2FCPn1aABTVVr%2FlzDWNVnrfzGkZHrUHrJ%2BvH0ZwmmRbBdXwITcL4g0Ngska%2FPoJkuJsTFZuVF9n1CaIiScYgMI8oFHTmNhKExBBMkFTyjkKQlWVyVBkTZ4b6iUq5P7ZjKPQMaUM2Ku%2B6AaoTAfvaBALn3GoP1%2Bf2KWAUE0B7NLc4zPsED1Mq7MyAYhZXk%2FkeqYQTxRZC%2BTyfuv%2Fv%2BT6dDe%2B9jFI6%2B1PS3lJJGq857AGpnOHLxCNrgvuszCOCLOT%2BAeI3bDGJPripQheP8E8tV5rdjBoEMmLVBhSclC56RfihA6MnKalN4qAO9PxoAvUEtOMnXiiZQajlsg9bQXsNPEGYel7cDqy048UzxvDbD4zrY6pK2tv3R79DVysp7x6CnqUbRmAajqpvHL8Tmk9itSu5V%2Fsd5Le08dpiL%2B1j4ewwZCsu7OXQ18CidTec1tXpqga0fY4Ku5CDCg%2FJPHBjqkAe9aTpzs8xl%2Fe68EVMpAF2sxNDISxylGtYlRBrpn2UDULC5Ekw5rXHLo5mKGvf5xYjIYd1iypdeASfBaOkzjNZwSXmzOq4yIQWzz5b7gci3Aj0JA6WKql3GFPK0gfpBMRgklOe69Uj%2Fl%2FvkezWJKXoR4LZkFb4Sr9v3789Oo9l5QSU1652wF3rzkpbdzNaJX%2FTFgbSWbyw1HgoLrcAkhaRWHc3o8&X-Amz-Signature=5db7c5aef25da77c67f98e7a841a6d67c1f9a94690006f4d764e7868aadc98c4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

