---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667ROT5GX3%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T100040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDkaCXVzLXdlc3QtMiJHMEUCIEWpEwQJSE1CbVKF1fXxIh39ikIjd107yk28g3K2hIY9AiEAhFQWRHpXPHKKv4EbIs4D9W8mjOGd57gKwh9wL16FThgqiAQI0v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCZtEdXC6wHHgHZLPircA6DOvhZSJaaV1EyEgNvlX7JY43%2FZ9KjXjaCiHVOxUa9gThEYxDjsmzvOp6C0%2Bc3ZMGlBr9U4ynI1GHPToGSBpoN3R2PBm7qL%2FMIBkBk9FjzUnoESn%2BalbP8ublGIXNhAtB3pC8qvOdvxkRYzAxzR7nl%2BknGjvN4CeTWT58n4xWAOFsMgzIsPkvOUNzLxZFEqgECrI9ncyN%2BrzsAWkdgJH8jhqIKQuM3i9O8cF%2FsileTc7yOWlX6H41BqCROUhVB0tC0psDAJY6YMmu%2Bekcoq1I%2BCGqWPHKtm2Iyl9btkr6mhZb0Fz32jmLz7NNym9aEqK5WzSfD8asNuHz4VehwzR3GPH6u1F2%2Ft3bh43hl3LlbWUmmDDQjTGjLD8NfNzkRkCgovWZe0mTbE4rGCphpclOX2HxaOIGuITLh%2BXHm4m%2FO0hH9fG1JGaE%2BhbzhZG68m%2Fpvbb9soKrzen1plSSFwSnAFNM%2FSXtVUiLHkIncCGK93bUVyVcqMyPuOMZ6ZJEPVjRRVpM24IxCc2B9v0fQb3BTXIpMe5WYLyNAjgwCXnGCPZQNdkJ%2BW4W6W7x3dijoLXqOtORBCiFZGL9JoXigAozaomz8WH6s0r4%2F1ylGkWvx1wNpKsfdsTuGLWi1DML7pnccGOqUBYAhkwzqYIPX1TG1pG38lYN1Z9VEACmKzLHhjWdgXuMh5MBOzdJ41%2FRTdkBiyxlgFw6iJc%2FJR7kekF24tJTk2cEskWG1zDtAd6tlyx7u3sFpVHDW7xe65ktaKBw6JKb0j2bvvBOWQB9qMZLlGehqPkZwhpCQvZV1X4Ttb2kuVt%2BrvWT%2FumMheTDxlFYZLtY6g57GYemBq%2B0LWVJcPaj7XmO9YIG1e&X-Amz-Signature=d7ba621646fff4a4a82d0ce73377572d0835708551e913f6d20e06c4b9f12cf1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

