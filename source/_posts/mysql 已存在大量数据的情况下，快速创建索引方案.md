---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YUWAPK6G%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T000041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHW9IRl6ZdAFuXaBJRtrxYagJ4xpcbLR8GssiRwCc70pAiAOKCOMhZ5LByDmarB%2Bkho1XzEJJ8ew1BZE0BdOfk9c8Sr%2FAwh4EAAaDDYzNzQyMzE4MzgwNSIM6RIV2dkX%2BMRDCE2dKtwDsQeIlnGczF4%2B%2FzbMKF2ZuL6O2HgkVgHm3aAgdgk%2FW0mVC2mE%2FBNzPatK8tKkFj2bxEBA01sb3bTnaTNEySeDnjTz4YprUCBAJ%2Fvxs0%2BsqbygbkbLWLOKVvy8uWZMsU6WwRCsz2I4nSlpDRuXU35rYsszFt2J0qJgj4t2qncDswuoQmBm6YpZWkxWaleKGwdDter66%2FeR0cpHYMfnqUAxJekMOq2lrHIxcnv133eBftl6b28J556cyEFgMx2bRMEWmX4GkgaisOuHK4aWOXayBc6NRf0JKOk3JdPFVj4uu1Vr0RRkOLFDxdApbOEu3Hz%2BicRIrvhRQrJvLjYv%2FyK0zals2WNfeazy1iCsAAdjv0Q%2FBQSgDZug8Jw1K099BPV7cEo7%2FjjIBXRR9XHH7i7awThoodKyCFZhWTJ7F48HwtewzevmwlmMerCBRGKZKxtRv3K3T%2Bhpi5yf6Vw9zJniEFyJKol0KFfqOUw%2Bz2GoMHX6sMpGrk9YAoch8%2BHfqDwHGVRAvT48Q5blRWBSqzhEX2Cm8AVzalh3lj7EUYYW3kkjSdAEK9GLm6mfIQIG6oG3Pd8R9pD3TYjcIXGy5bYqRt7x69Z%2FkGItLaN7ZvENnkwFj9e5Tf6YzhKeg4Qw1%2BaYyQY6pgGUMnYmwvjxAy4upsctGa9QRs6S7wpONh4BbiMChWgGFahMPy0BATG8D%2Fmf4%2BQ88BUnn5zu6UFHgKMNC3Hl9h6%2FuhxAG5FV5olrtzb2e6B62ENHFC6pLycWLso1w0i2F%2BfZA8dsgpVCoa8BjFQk1BMun2FUvlq76gCneTJXdw%2FMquy7W%2FfiULeR%2BhA1TW5SCPBt5wS5DYh82UEY7TmTFGtsiPWDZzF0&X-Amz-Signature=20ba5faa19c5b61af296417754815c9a8c287717d0686519f3d89d3cd8b5441a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

