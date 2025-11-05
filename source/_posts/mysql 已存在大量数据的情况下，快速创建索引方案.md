---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZXXZUM5K%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T170040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCBlgyPOKj1WLJ6SPqJZW3RmSGIwM8TmI%2Bw44F9f2k67gIhAP0cgKet8YZsaiQVkef4XrClbxLnuiqTZaW779c%2BcdmoKogECJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyaTDMkILzinDrA6WQq3ANwfVs%2FAeiUGOKah3BODqdM409OqAUoK%2FJLHjmPCavTA06GOgFJ3z5YbexclAWctADRJ9UW9vXUG2hvnVts%2Bscoczue%2BVf9kKKIeAoK4giwXe4XhQ%2BFVvJk91Z1iKxLsmxvB0htOZLFav%2BQi1Ah0ivAtDhN04gE26Isd7JhCPUOMiMy6F3GcIL3xnJJWBsCOoRonZ0vDIk32eOKS9bEOcIkf6p3TGGTugAhp2SbhlhUy6iMyuEApmNG%2BQ%2BVBtCVF%2BF3A8wR8xKJUWz5XnMucJwufxflb%2FHoTcsNclgcsQYNYw0Ea7CJoU2YX7iG7GU%2BneBkJ01s6fZvTBRotgc8KJ144Bl2klGniLlDfKKqjyGv9uQ%2BeRWum7IGRLDirg8LdIWQk4LEfMr%2BuVSKm63RmgcKQXHGdHAoGGMrwV%2BTMV09hRQal4fd6Rzagl1%2FowlaAsQe41uSSQIUAQB2KT%2F%2FuQfyXrqoVgjrGK%2F4TcMH1F4LfIejfgZyFJNh9C7QbDoRtc7amljBjMfC912bn2%2FccObbNRg71KWr0Lr4LiOZD4Bu65STU9HwMbiQ4SKKXWVzcLAYuzGIiL63hNfE8AsB47kHeH1Q0HnuQo9wCPjybDpTh%2Bx8IjtNyMQau%2FdQcTDx%2B63IBjqkAQWvasogboXjLP3hcyI00m3PwpALuKLs6K4xUrPbG0OHUBb7W1ICOygccS6T5hLmR7vvI3HHPRs27LLStMNdD7TEXvo9DMLyDIc2OOE%2B2ZUttnFuLG68hTDpqA4C7LZjPPUylj7OdpaCI2Lt4O38a2nAxgUZ%2B2QvgUNltZO7OFKeAEVJOdgVF4FrGySc22XFK%2BwWuIbpnaJRdmxY%2BGGxkr0YcOIS&X-Amz-Signature=f32a8c2a68a912bcd3a4f69d94eadb2f4329a1eddcbe7e8d8b714061cd25a7f3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

