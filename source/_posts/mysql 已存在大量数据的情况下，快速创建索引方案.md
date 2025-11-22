---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QLY727CG%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T060048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJGMEQCIANE5x%2FpCTwPlaktxD%2FSpMUaPal65Bt5p7yLU7MjcT7QAiBpOQ4sRtJ92GwN9JpfnzyvBZzWQ1sP9Ek5ojwim0fyoir%2FAwgeEAAaDDYzNzQyMzE4MzgwNSIM60lcRx39CwZUyu9WKtwDW5elvCMtKXIudz0CudxRAt9VXtKXba3fXHLj1XhEUr7j5H%2F7pAUfwdy2onFoaz40d8Y7iuxk%2B7G49lh3UhSaCFcbZhn%2B6rSUMf8n4OQd5DBgnunSrTOlNdWAFXeV5lkyQqsJTqgNwmk8gZJROft9d5SujukaAHa%2Fk4s1xYP%2FUUTd7EKUcXao1XIogaFmX7sQMzt05socbgz0D48a0%2F19Yu2kDMvsCC7jjrSulNsuej8LlnXWuqi8NrsU1aS49fa5i8KimsFVFfkXB4tOxeBH03l7lW88g4uQOW0y67JH6icdjjDv6ywxfTtxhQywbM1DBay%2FwSsInFXSeMFrvARpLi3s8b8gJaTdQN%2FUXMeoTyky2yhVuR3v0I3ERWScF8FBPGNdz9Imxt%2BV%2F3aw9iaSiKIKc%2FqYBbXWGNpTIfksplJLflGf%2FoFYPglCZlj3dVFzYVaiBGwuNPF%2FEDFR6zck22ICzxJLA4YYbGzZpb9yqUcXwUfhuo54yM4kwEo9pIvXcBr2yE0sWX8bctipzOOXKYejfGuv3muNPHzQEoQoTwsU%2FzFJlYSmZK%2FUFDf5GCsok70EWhcniouNs0qSsZtTuy8HjjpFz5xHfcY6hX%2BvIloT%2BuWVNBrfW5IzPb4wtYOFyQY6pgFwqszT6rwc%2BO0or%2B5GXxgmDh3AX2l5mHph%2Ff4WxnXDquJorpk84pfVayULbVSzSYofrQeAlUbTgbi8b9QHtKZMQlzY%2FDB%2BZ8DLonUhguZoqGoRbabVQMlCxCRo1jdpIXr2FmjrS7n02uNPfMF%2BnPXaOrhKw0Uaq25Kt%2FPEO6v2lX7F06M8eLBXK7NmK0ZB%2FmaNERZx%2BunXQuawbgpE3C1ZDxYB%2BBVl&X-Amz-Signature=386404eee654f14e30980e5e0535524e37f99d49df019fa293c2c6802a49e225&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

