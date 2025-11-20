---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665K7ZX33L%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T000045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB8aCXVzLXdlc3QtMiJGMEQCIGA6XeLhuoMc3W0c4dXWUemqndsxlnvhZTb9XBYJTb1AAiABIvkZli4KPhOFYYmjkAEfeNR9VbxQLG8jAb%2FopuqbjiqIBAjo%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM6nUxSTL%2BDS3vPA6mKtwDK7pYToGfYyLLfqZSG8z9gt4p2LFp41HyH7yk2e8UCsOBawgOiUv45ECwQjgmY%2FeB7FoyPmcS4nKwWC%2Fo95nYBQ81topsZFjDTrMmVvXHAjGgEnDooVGL%2FxJDtXaAwsyTGGW68JnNVbLb%2BMiIxb90fkuPMhgdImk6Edou%2BU58Wo7IeKxv7mPXiZ3AxLagJlPiUEQwq49c3fEVTsFH3XM%2B5ZOZt%2B7cOClwr8JxZwEgkiwM94IgJbNFQ0SrMtI0ASFRU1hbqtcO9TtXA77M9gj2o%2FumKSVXorLfcueqd9cDVeRSE1qOSz4LvKP1Z5Nto4N8Zmna3IcQD6ZlweRRxgGeEo5PhEM0h5T0SK7o4ewxNTlqVPJp24wYnHaKkFFdgiUA4dOUsKob%2B1WI54pdjIHVwMY3JWHuChX2XxfC2iDPmcU0cvxDc6vJulGtE6LM8uRvXtahxjsZFd8SCvO5xqwiutZs79TReEp3GuDCAmr%2B5OwWnnWWPWXJMW5ubzcVC3%2BGjgSHOAlpeG6C55z8Cyzv1B4xxLxJnmmVGy5oDZGAlMoEQaGNrUqQ9NcZSSZV9klYJoHN6vaRu5jSJmmLaf5iRKbjyiMCH2ECsibTwNL%2FE5HB4Zj3p3jbuEnjt8kwkJn5yAY6pgEHm0O7rcbwAmWbPDDSINojgixDyr%2FWVxMsrbZOCNoSN6y9ONEYmewRPB1jUfBiPGpMCVf2uF1UdXsd2A45Xw8sA1Z6CsRNznEQ6S3oLXr9%2FUhT0xwb%2BM2YJL4LX465KAN5QPp%2BUi6%2B76l%2FaLqJo9T9PpCBXof7iaYLZ2iQknQG7OuHZplhQ6n0UFvbB%2BaHpYGn630lz%2Bhvlc8s4VhEBNDbOMgDIghn&X-Amz-Signature=6bccfd8ed1b2223a137de8b66fbc94813f83e5850fa8e3a200544fc9f84e26d7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

