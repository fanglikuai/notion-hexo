---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663MRH375V%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T160105Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJGMEQCICxJJkA4jAbZJxOMr63ad35pZlHo0eph9bkub3ErqxghAiASxzNOrMJTSGojHx0CNmwCfx2Rwc7PN71puUutb%2BHCUSqIBAjx%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMX7WErT2%2BMn7H3kzQKtwD6mkJ4yLEFAabILonvHSdnQXkwQOtMwE9AwEedL8Uu8OVjs%2BFffK%2BMrD5WFBANTsJniOzVthljFzXf1YM82RxaLGiyCnMRV6VGwi99u76G3yuIZVrQszmniXlkzoNv22y9H53R61lnYsUCgw%2B6m95X7l5kvVWOeefB42PDuaSvh%2BMCorwUCHMR9spETWesICfPiyVvJkaF2PVig8Bev%2FKo%2BFmnZfDZV7uQ1plFsdHJqPMoGhhaLjWYHoaC7rA%2FtLe%2B6I1REfbKRe21LrHRgMoGmhCjIZgEefX1Ty2fLZUJ0L3CYyVuZ%2Bmhe965mQ67P2lIVKh4gB8VAmfJD1uDtZGmWsidMN25PtIZ70SH5CF7v2QJuJsXcM41wCDeWDKPYK%2Bom9BaaEifi0dyKOzWjwDEdvrgx8yMYThFn%2FpYhFsMjFz4M90qsY7DyU6F4okUdHHHsVcZF0GKIqQyU%2FRDLaJfXdalrTrmUQ4535UjPgokw6q%2BeIkEQbj7R4KKIveUH3LuP9qtiJowYmiOiMlS3g7cFQxY%2BvObGZMZslgTa6T%2FQLH%2ByMvml%2FyqZAqCDw9jwnFZDTB%2F4x5YzYBmOTpRIUhRVeAKkVIZLDjj2SvDgtlDAmEvFM%2BnPb1LU6q7NwwnvrvxgY6pgE1mGQabyBBn53og29iDcoaIkmu%2BVwsXhM18HBMcGBSnptCiIS%2BUW0ErmBUb%2FLQ%2F%2B4V%2FraqNm8qFOhPzfEO7YzaNAV9qPXKpJlbkDI8IsaXhyZVyLGIXyFWxAiGlacYZmWx%2BaWfZ8TFEW2CLZFyNZFuUFgWKWAlLp4QwOvf%2FzAUqoHtjAqgwRj%2FSUKoOOj4LeON4GeWK1OlgPcQ5y6jEn9%2BA26BtEYQ&X-Amz-Signature=e9f57986bc501f12df4a2fc9a148b62e06a68e7ef54e1f02ce1eb10aa6175e62&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

