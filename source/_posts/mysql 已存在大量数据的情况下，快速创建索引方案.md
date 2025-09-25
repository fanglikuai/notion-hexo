---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665VE3IT2N%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T060100Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQClLMdATMIKvdmiFtdVOWgSn1UiZVy4pPhopKLMd4KiawIhAJ6m%2F833Fc5EoY6UuCITpgLfeOmjLDTGp8rCQrWnAOc8Kv8DCG0QABoMNjM3NDIzMTgzODA1IgxsufzjMvsByut505Qq3AOT9qTKS%2Fidhe7uT5AAdFfEzRuml7v3ej9%2F6QRigkjIXrP9lFEENSGgNjC3SWYEX4%2F4wkE60c%2FEy551qOw%2BkVKljUiIL3%2BV0GkBWuPR3HicjcmupAJ0Yk3jzwfyEZxcR0L46osQywcie%2BbPlTty5ETieqCGYkeP%2BFoB0Q5bABzb%2BVaGXfTHypSRlCA25npgR4BswyC7tkQZ4Xnf64%2FG7a7SQy153zviJ09TjMpl6a6aFj5AG0zzSFlFEHFNXSN%2BrKY1gBOED2rQhrHpRzqheIYkhTx%2F%2BtNR7o4P29ecVvFtB4lICvQgL0wif2PjEFExKEF80%2F6QiKCWQxBZ5xTKkEYQMCTD9IBoLQ1GQvYz%2Fwe34WSrAtgJJLtfpxOcdFzXMcJfKiBEMmXXbqIoqU%2BirTwU4gbyfoujCnAvcZ%2F6Xix2tygSmXyYKJPfcEhTsAkNNU6zw7kuCN31%2B7Mxd5FNx5kI58kFqwxtCLc09iIy8hQSNp8fkw1x3fyf9Pa6IAJacUVgWGirOfzK6tsH6Lv8bSD%2BaU8HO%2FHbSHTlKTYbAHCNVSt2M%2BujZ7UQcf8XGYy7W4NBQvcHydTrppYLBq0wPbzDZFlJXDSjpda8WNCPnTeeKEE4qKdGLs36GgNC2DDahdPGBjqkARSuRhjajxnVSUDzBdmqLXKSOpdXQo9X6gBkgxSxywEuULnizLS8Wy5%2FsX4FmwyBKuLQP3Sw9v%2FKtLzyhB%2BWsAfoRNbfzlahgtaCWzfwEJ4eg9GOtkUmu5MJntrVSRaVsbKLPByIkRgkXk2tVg2AvrHSpwUrR1QvArwWflxOOWfspOLTRuRej0BAVWsBnXS%2FWTMedunwpnRa9EJj76M47DWSNrmz&X-Amz-Signature=c0d8172676e35765311432a5c9a2e6ade4c181e17903fa888add397b5a67da8d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

