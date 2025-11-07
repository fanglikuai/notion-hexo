---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T6MGAQGQ%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T210046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGKtb5CB4DyJIyqmzYPeacpoSMqGfDEoGR7Y0VOGaColAiBClw9UnFf0Bum71LSKQLdO2xxjtO%2Fv5rA9%2B2wnfFykjyqIBAjG%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMpDAWbqap%2FbhO7yAJKtwDIha9AAul2Dt3OmO8BukA%2Fl0eFvPaX6wvpvAmdCQL1sj1xfRtoFxEEkZJ%2FLsi2e%2BqIZnmj2GoJKjamu79HgPH5l8l9dm08WgwGtBnJv6YIeM1EwXTLuC54sPzq0eDugXZYeUodRLXUqDe0DPzuGMV8I4Gz6WgrsfOpRRYdm1FQent%2BEH7mQOxW6GfJCl5%2FkY7rlvHXi8CgSXV68Z%2Bk5QUBxUCbBtM5qYX%2FhZ9m0JixhB9dYRN1VVO%2B0f7g%2BMzqT3LmtFRlaZt4pcl5j1hdtN9Y0%2BU1finOJyTHgCwroYC1rYwGcEHTmqhcJl1jSX4g4NqtdQYGGxsIuyBMO6TC163g2RqVAEwlcKq5E%2FFF4VAtJOtBdKv5P%2BP2tBqWJ%2Fdo3qaPuoq2srflG9vHVj76fCdRDGip04Wyv942X3xAsWwX1VUrRlyfpQHGjrjJ3rwnCNj%2Fn5HDTud4UHlBgvh2F8xoDA1ruDFUPT%2BIbpzcqX8EDPvxnwpdlP8U6qvaK4MVsmKMIqFnJbfUt4YvTv8zGfbVGC2%2Fdec9bViGvu4oYvZbqdMnic03Wp%2FEBgRiDhpDhiI9JqjfphomNVAJN3nsaVrIt9JBBTpknlwT24P49vhoWqRUHPoDmXQ0oxU14YwiLu5yAY6pgHUqcWp0SN6zvehRvRm1YgszeOYMLMxFkQSVLdnZwNyzDdssiv151xSRsqR9FQlejDGgI4Dj0yJ%2BsspkztCxPakBvkXMsj%2FfeR03M6RCgy0V63%2FON86twYFKPP80%2FsmMEgu4qVrRxrO%2FGCwnuV4n1bSPfVtwe95KYz3mUcgiyyznO9QnTvcPScEAwxq4hqvPWxwvAXULqDiy17R13wkDicK7GT4dUYp&X-Amz-Signature=fea637a7ec4949cc33cefe1f0480ad05653d78857373ff00964f3d9e86dfd4d8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

