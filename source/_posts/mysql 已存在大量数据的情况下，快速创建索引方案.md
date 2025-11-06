---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667MILTOEL%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T120100Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEO58LYt%2FFFzUydgFUx81mnp%2FSUBASGi9czkfuksfGIkAiB7ch2OgLZ5LOP5JpB%2BePAHBZq076EG4wTRKCuG1sqTtSqIBAik%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMP8goweDB5vTY95CNKtwDxkUjHvD5%2FtQ583qzGrTXDd9ZXHlQaR87CqIiU4wqmg0H8GxW9MALHHFp%2B4BarTU6A1ouJTMeTFgdUxa7B7z3Y5vjjteilra%2FZNZB%2BpzH8KvNuPi45Q%2FiT4Tcsvzch27lo5nW8FZGp0%2BlgX465x2Ox5%2FUAs9%2BplMGsCpkKlkg6%2FI8HBXKRLZPabVQQRoyWGSK%2FjKc3lXPih7KUFj0rox%2FtspcRdDh%2FLxodL7kFEDTo%2Fr%2B7sGUbzKMN0kLh2%2BQ8ZDQ%2BxcnHwLkFSL9g7ru39RvYESBYsA8GdmJkIO2Uxf5y%2BDAcM%2Bq8QbOfdKv7qbprAd%2F4LZTZ4nyRjy7iA47NIpljIrNjZ4RBRIa77dnP4S5TbC3PaCgz%2B7ow2irPL8BcKP8%2B9kofW1SWSW1AV9g0x6WpmJJADaXK%2B5SH1JIxPkN%2FCacsrDuIUCS4c8TtC42atrZs%2BoVi9p4bfeo189K6sRqZqTQo0R19wUZhKcRPL3vM%2B9gQ%2FKrUJAVjlUNT9W2sgQvMltnjB29JGJ6ZoMIKa0%2BLyNyYj7GSQAjAzFL7Wg3EBfBW0qvnYbpLU7Nc4KFVfV%2Bh42NyyUwX3W2fZyVfF4PfA9Xp1CS3WQHGZRwGXwD31wsuEc9Z3ncFcvDEdcwtYayyAY6pgEHwtEhXuRhmrzrHnvc%2Ba7dv0HwdCvBJ5B9uiUzWDV%2F2LaysggS3BfV9G%2FD6Hs8vKC6UVvdlp3ppnxQOJ%2B%2BHgZvmN5l7KDzTJ5sxslExzZQwggIqGEAp7nHty2xtBGF7v%2FzYpp1fIZm9HgHuKebQN1rVurS2hWg0%2BrD7Maca4dTH3HwVhbOrXui3qGfacsxnw3FqvDVqcjWMMTKfzQrNJVL06ibguiE&X-Amz-Signature=0aab051191e43aa98239ae43dc21fdbcee9138b3031e3e583154176203b42654&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

