---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RQVZZOE7%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T090051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEAaCXVzLXdlc3QtMiJGMEQCIF2CbcGO9az%2FRUREwmdwFayu0mNt8l1ZjZLChKZqlDflAiAUGBGrVFIEQwt7FXjJtzCdO8453Wmd3zUtZtcZ6kh88Cr%2FAwgJEAAaDDYzNzQyMzE4MzgwNSIMKF5ZFQs7tPKFpO9DKtwD%2FLclcr8zkql5ALCv6%2BKyoadAHrQCCS3OrgcUwB0NhnPSebL0DhTt0yND0dZOECXmaV4V40ZPvgu7MCxfYivzIa1va3wJp%2BjG7C%2FS5to4b5au4Q1BeQLX7g2BxCVTYCQ4hBbV2K4L3DsN6bbc7QTdGkAxJfXJESgE7vexf72VqB2I84ODowrN9btgND5QEVtGVa1QPzDiKPSENsSw96rbNzsArW6riVJSTywjaiuMC8kcy9BvqfVGPgOWXQTM6Er2LsTrzMammgN%2FjX%2B9fiEk1JXIMSn5T426tfiDS47z36tpMbyL3DYkIDkKpPVbymiGg34pFfoC1z%2BTcpiFeYDbYFhKe8mzU7x9vLrUWzsiaysr2NneIe8whhKKM%2BFukcv3nZOGHZ%2BQ0hZEVQTMVJWq7RDZNcEQP0iCc6iXanB%2BTIOjOTCVcer598dJmviBsXeGNXE4NMbthMgaJx9t1%2FpMYmMVjjU4q%2FgxxeQ1CyobF1MrNiVNIqDRD5EYa5B%2FKVZQDINtrqY1N8hiwUzN34%2BPx1CwKTkUKJHGZyTY3nd9pMx69kRUfRroJVCBj4vtodshPBSXSVazlo7eWNKKYsdpZ2ge%2FBfcIZVPK2mveHNE6OF5L3vIioYsm8fMw3wwi76AyQY6pgEYCWGpvpgnVHXc1cInGgKaFd%2BXzVf8icr5xse4QWEO4XKDkMDrI%2BGThkv2vAHOmHCVxbz%2BIo4R0tSL2Td5LL%2Bc9sDxYgSTcIvHMTkHntoa5nlu5HkoY6Y9DkeBQK4Igge%2FmExUAWgRZqHK8yfBODSx7gvTNzewmTwlO6G4sPww%2BOGs05I%2BHTuVzogCUztw30qKG%2FZuV5KLP%2BGcwHwBVwOAnuFI1VW%2B&X-Amz-Signature=17205d95f88a2d6e555c989f9e60d166d6e123924a72256129e54387101da7b6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

