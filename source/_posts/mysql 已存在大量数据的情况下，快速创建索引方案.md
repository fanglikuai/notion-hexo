---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XQFFFJVV%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T050053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHwaCXVzLXdlc3QtMiJIMEYCIQDjFxSmBjL73KQULAr%2FhH1J6qPIDCSF%2BZAIVhP5icDGVgIhAIbNdSz3kpM9zoyWUKjayUua3ZKevOg3lXyPVtsWK9CGKv8DCCUQABoMNjM3NDIzMTgzODA1IgzeeohhzQNTC8VfE8Uq3ANx%2B21PGjWxHIDvBZ4gmvcrCQCFtSE50d2eXlhIS%2F403T8BbK1xxvb44Ao1soJbKC%2B5s2fe89L03rJH6vAAm9xe3hEhwIchrB%2FbtHtMd25wWFOwYf8T9oMdD7%2FSWqifkiYYvXn5AnNxBz6mg0NnLU905qBXN3m8nV3Kb26HNFtzjUb8lLNUFSAoCAixPfmIMkzIGghk2CtM%2FDGZlvdWVKf4HkBXxp6Gi6urxCH6D01e7Sdnp7rmwelbqSPXHV%2F3emGIji71dp2Qzj%2FsnWYYTyR9pcVc2yO%2Ff0yffvgIiyaCX5d%2Bl%2BULM60QXvwFlWgjv60Cn9vUpUCzBMKyoD2zvvL0wAjUjkzE%2Fu9d8d7OYR14awa2KQr4AIyZ0LBY6RuwdR%2FR60uq4sPSmeMXsvjvqWoINI%2FCmkHo8yA8czb%2FJp4GNLuM5IfnCj5afUuVNzDs7T9vUvCz5mKFAHhVeEJcSQIyNsmMoeHSt7kfHFV0PxAaWYxx11411VqTTVLCtHaMhYuE8g8pmicOFRnEvBfEWzF7PFIAbKtcA3XAuY8aYy9DFIa7NAWmMgD5s261sKGKmQAU3JbTmzgtqQ8nB5u45WLcFkYMR3SBjMwgyiy9l8r0NsmXIyL%2BLoqhWXSNpTCZzazHBjqkAeSZV1LJzDas1kdnG8QKMJZS9r%2FJXl88TLnyTixp4%2FmqE3ZeemwWaUGsx7JtisiipP2irBEFMy1dY0%2BC%2Fibnj8PuHgut88en%2FcIqNoIMBggtiFNb66ZpqIt0EDMQMG%2BUNMpbnQ4FYI0%2BmyDNf2u5vnIXBAR4n5HMr6htePmo%2BmVTAZpPvS4BmbBfmSGxXcrVSuGURvErO4f7kr0ITg2tCzgQMzqZ&X-Amz-Signature=f600756c20881f17febf25e02f57a16a25bcebd8ed5cbfd573fc4a7049f41652&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

