---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46625ZVABFY%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T230056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGcaCXVzLXdlc3QtMiJIMEYCIQCiFjuSe%2Bq5hXT7F9Ij3W33OIYGz09Wv2zzdwWH%2BCav9QIhAMlKP53VeEy83mh0iiTZsnNjKN1GmmFgjaMLlpTSeBHBKv8DCCAQABoMNjM3NDIzMTgzODA1IgzJAdfBODK77NvFgbkq3AMNSS4lAX8iuafQEtJVAPajUVzqdbuf7wWeXZzLtigPgUqTSTQO%2FGCJ6gIJeYvNNNFlw2B297iwVvoiggKKmxZlLD%2F3mjzSUaMdnkydlDQkOEDVJuzhrfo8sHh8ItRw04yKyg2d2pSYF0u6joXkqkd4%2BKxMqxx284QJzkp8vtc5GdtzA%2BMy7hxX1e6SILnThZQWE%2BKjRYNn2Z4i%2BIIHIf7ZSu5EvMtYYNEZwnovSpBmlEyzsFySfVgUq4st3i0IZ%2FFxEey10JfSzlqNEbzb4GRL3x%2BK1LM5WbKv%2Fjcyo4Eks4Ph9QEga%2BaSPHGd5k1FLKm7nMUGLEOTqj2CxPk%2FD1%2FW1X%2FxUwXZD5yrOnz4ybKmrPOHPEUsHCv9ryGToU9%2BIDDYZqXpB08dzPcGD9sAZbTTatjo0Y%2BqXsaEjcvQ91EWOCXgeeM1iIOJ%2FtTcwS2YxTCnfHOgdeQlHSkcIDgGC8OfSx3ib9%2B218s2eA1O7peCJQn1sSbO0Gb1qZaGYmucS1xMYHFhjUWon9%2FRjLqfeCcm1AsKMdwp1NsaTNb8aNxasEjceOezQnk%2F1zE3zOn2I7Wq3lZKtV6BxFXmNtjtpuSaq4KZye1J5X2ssLDbl2T2tLzkprtLBxUhftjdjDCWl%2BDHBjqkARi0Nh66MHwE642Gcxjsg%2FsI1wmWLo%2BwgPnTzDhakOmPtSa4n5KDfI3uR%2FRm3rhBNaYTuvOyAn9wh1Xo45f3R8pypQnnytv7CLpbKocS1iXP0KIDPg2MJuUG0qsl5wobbVKibjWOVMiPOoWWhGcm%2FG3vASf4EHx0kZAlpbIOy4B7ZG6gFWeISFFoIUgHYVc80tfNPX10%2FWIJLXZ6oqsR8rhzcJqT&X-Amz-Signature=4891a477285e55795ac067c4700e87674b60c81654ac71bf443136070f4f2471&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

