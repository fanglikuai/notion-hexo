---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TDBEIOPD%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T230048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEcCFbTr9ekUX2i9WY5M9aGevPNTB5H0Tp3KaK0W%2BqkVAiBqg03V2u39iO5fkjo%2FQ1s7ivh4WVf5BGHXxAwFtrM4VCr%2FAwhPEAAaDDYzNzQyMzE4MzgwNSIMyDIvo5E0ygU0to2ZKtwDDrMAf5sWdD5EWgCeZLv5coD5UQ27FZtISgxoscikwFP%2FegjQitTdbxj0xyxezhyI%2B6RFvRxriyFiHCmgrCO33ZUjT%2F0QCnY4b2ENPCDrdQrTU4Unf9jQXzBuCIPxbZsC8HNjMN%2BNFIpYZtU3ZH0MzbaA1%2BMZYspj6IpudQlCpxddCG20keLgcjM6nkIjPAMyX8RuNhri7zGT2AL%2FZ0Bx2EpaLhJtNYfRr%2FIolI%2FG84DFkKBQAFM5q5MAr4%2BXWbUThm4dKNLNBR%2FAAV4PEBOC3ebBpKabb5xAY%2FdDRzK1ecNX3W8eMDw2%2FVBK5T8snBruOUqJzKcmDoRTPBMd7DT%2BIq86KmFqqwquIZjKZJloGlkgJXuXi5caZCJgToPZDLgb%2B%2BcEpO9BIoKVv7Ojywhy9hUE4l%2F5o1ArwFKfvp7jHcgWwWjszvsSZJIMZPMAJRN5VevDgkp9V5gWOzTLFitcMCeZblXeYjcitCjcZ0Yiec5BpVnGakU3hwmINjO4v7NFxoW0unZNri%2BRrLcHxm%2FSbYgF2x1eBsg%2BAZghKNQxNyUmcsW8j7ga8UnJJMrw7Lyw4epsRGQ51liOTsx7ZsXOHKqIa3pZe0gEtwB26xQnZmY6%2B6gxn5G18vZDDjMw6o2BxwY6pgFtOFM80Vqn8J1px9MeyvVWXdRFDC9uMIoZeV2idJnv71UU0z51UaqCqEbWoDJI9LafVPxE3AZXU6zFvFlaRaTt%2BnuQqHkJmkjmktqzUeJyvvQFJiF9vRvK94%2BAHMXQDK%2FEmOxWvcADSJqEqHahsWBaHZFPiwY1JXrFrKuBLqHm70%2BTz2XL6rMigsZVZXRdOukszzNF2gMkHE1IPKt9C7J6Oh%2BZIop1&X-Amz-Signature=fd5d2e1907f58144c08ed0d7ae4b5b1cfc5c99250664fcc0cab104b5b41dba14&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

