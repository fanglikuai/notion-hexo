---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZVAJBQHJ%2F20260324%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260324T125107Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDRRESIxO7PQJFnAEgCj9jaBiN5kb5p0Fm4%2BXIr8Xyv8gIhAKcnIFF6iAqseeUtOLoz0KyY5QajkY8aQqfj4P7%2FlxYzKogECJX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzkfEyD04%2B3fnm3qEAq3AMOlSUsYuV8UD7gOU6XQfL3ilkdvcr1U4O0BewG8RTUqf2kp%2BrM9sZbAv0ISMuYexNtZRJi7Ffh8YLyyIZs%2F3JWgLmO307XZlt2DGmfAV3wDcIYsn3ymy427sW1fwdUGzUU%2Fi9VSw3vTb%2FAi8aypbwJp7uKOSaxp1mDgjr18QA9ASKpXYgPMfIY2u2VZvhh6%2BvdQsGAXDazQs5jH1VBjxPyLqo4dugTELNJpz8E1ecFK1XLRnUz8sE4Ink6nYWBtthsMHPYX7%2BsOjJuoZgLQ9IjEZGfGtoCKS83mooH30TfQJobLB110h%2Bv0yxjikJKanY0eI%2BupnpMIMBMpOo1ldRbZKlVA1TgAQbV1GhklZ7gwaKiSkfHN6RBVrNHsG%2BmHVOjgrnWAGqlJYalapMxo%2Fwp81JaUv6OohIkQNSIx0Ggb1xspI9oIQmigUcnnsvP%2FdedJF3yo89egC5xj%2BHrOUB4QbwH0IdCAG2mam8S6IgU3QNEqtp6NexkC2lf3IR33aVZ9Vy5LiDNQ4OwYV6uog9CAcBsXCmpImo290c78UubJlZLmxnqlD33QysCi%2F45B96Td%2BB1%2FeuG4GHszqqDKvnoJF2jXXkK%2B6qMbCKjolj29Uq%2BldcJKcM%2FZ7%2F9WzCG8onOBjqkAWsVoK1AnJaLPCxmXmHVt49JUF9aUdgFguxK1s19CipEE9nQn2t0uKWIZJsSEm3wHUEBvUNm%2F3zGVZzJ%2FsIB8mpt1weB4qwRCzIP%2BacFtO%2Fk7v%2Fbx%2FJXku%2FMoa4IVo5P7bD1cZYRFNTAZz6%2Ff6D0fLG6xWcgXK8b%2F5wAhoCxUPYnmiNueH%2BNn%2FfMjYZ3Mh%2B7e4mhsxG9AzN7l9ROJ32h0NQCUGfr&X-Amz-Signature=8c4d91f8b35297ce864b97560f407f013ab0d123068b5b5aacfbad25e65d1dcc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

