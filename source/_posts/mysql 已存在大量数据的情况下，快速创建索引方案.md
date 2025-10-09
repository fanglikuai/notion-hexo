---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SJIQDX26%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T160235Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED8aCXVzLXdlc3QtMiJGMEQCIHYoWyzltOvEeGjHXUHXreOgDWqDbF7LByT3Kym%2FAm2xAiBi4KRJxfK1IOu%2Fr%2B8O9QDjdWKeIOrrWNmwx9HRqthhiyqIBAjY%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMylFC%2BY7ZRaDenvjvKtwD5u0gs9OP378x81jVt2F24IbHlKTR9PXrs%2BWP3X7UFHAlG6y3U0SgJzfy4iZo15MtD4KQDdcguG7%2F1%2BoHkvHppcu1nbhqul91XNV77FMS4mo0SgN%2FxpUbKKlRyKPR1sLnnqBLvPaNf1V2wNu2ZmQpWUXnnG%2FOqEe5kCau3w3cL0uRmHIXyjEj1ChOcub3RVFroGr1yuE8CDOTuL%2FqyNGWy0%2FWih5xINzBD%2BDE184HfLPraaLNqLj1CdvQbAegyjh4gu2kWBRMpRjsQil%2BKrY4UvKxbjbBmVbyy91jtpaBexiR3aYmKfdn2vsSLRPFb%2BAvXGCY%2B%2Fg1vdM9vg6gGo0gC9rOYJHqXwwQ4kTaTSHnX5TJvKWxO9hniysXq8%2FHgfSA%2FTRY64cCY63xH5A9A8Mhxjc%2FZDA68AI8j9ZCyHgalXmZKITQOxfcck1ZloGmwLoxmsz2CkPCu2QLN9oKjMvIgMJprCF9VdYiXlc34rnGut7dG1e2eCQpzP1wWpudcXcQ1%2BfyMAQXwDOkcCcyAj%2F2oVReplBAma7kyS3V2k4ASurjvjDaRWQxL1FuLkhFHWTQB6hcRtO4d4fTJF9IeJgWuqEjB%2F%2FX4dGbDwMXyDqfBEioABTdLVdjNn6sg3MwmpOfxwY6pgFgfq1EAnzKifjK1u3CsHG6%2B3hxhF3NiQ3Qh0ke0aLR8Kru2fwIowGCNg9iumWXCGWoUYaB0A7fd5jrapIpYk81Aasjw4bi1er4%2F9ChUfn%2BQ2%2FLcBKjgpkg6xs95s6ycLypDkSjHmun%2Fv6xXg2PRFWdXMPTQUobDjfF90IT0O4mPb50nBufeAvrzlfKOlS5cF5OmjdAEies4LJOYjBuemLzGTyYbdgB&X-Amz-Signature=be04b432b68b46b714530804bb6efc1cb55057e1ecc890e0e07abec49c10dd4a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

