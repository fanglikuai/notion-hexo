---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WTXB4ILE%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T100053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAzs0ll1i1EMOqjqF8H06Ba0NYR%2Bhswp714EbexVNJzwAiBCuqfao88YOXKFTckss4n9uO14OCDm9ZAGJyLMUN3dAyr%2FAwh6EAAaDDYzNzQyMzE4MzgwNSIMxcNVL7xD6K39mxbmKtwD%2BmCe%2FPVkXGyD4WXLjda1c8F8MFnkCk9rYuUQcAkO50DldmYIDYo6xSn6LG8zmrWcaeSUAgqyo1QF5NiUWkAMbikX5pBumNXOgF92b8yXWKOq3Ikd%2BkIhppk2S5JDErj0MsHDDvqmTc%2BQ0x4uOhDmwttbqaiHhA5MDDA5XsQp3%2BrzK1dwRHqNb%2Fvo%2FaypwIlDT%2FruI9AhAg4w%2Bxw%2Bpaz1KxbzoYmsy2b6nKVa9SJ6NaLUo8YX2FjJGGIHWOzojt3Onb%2BA0yclnG0ZWSHayo7Chv5pTsq5h9BfJGThAdwPVzuLxvW6h825hs%2BIgTTvZU%2BLM3DL5VsckV6luE4%2BFAYQXhHqAN%2FsSnausxIoSryKPPJQBVoaUmSogPTUAONvKUqbVbCFpZFDiyt%2Bl3G8eDdHikshbY2P2LZUdguecqSKDGse0Js%2B5jb5jO1ETBGBgK9qHE%2FYoEEtzWmVhsxK61BkEXsgXF6Lp66ZzFoypo3rK0cYrFLNbrPDkEYM8ipiv3bab9u5i63RJs6887RSMyCL3B6gG5In8cJXA3YbPDk3ZT3IkaEXgdjRLrNSTwakA1fjImKRqEAeEyA93ZEX7UIg3iniVNH8Zb7IwyRIFRGbleMFFPuPxTKM3T7CIhMwgYPhyAY6pgEGH%2FA9tjRxBmNXdOF%2BmrlpXWV%2B5aMaWLeSzyI6Ipc%2FOdw%2BwCfR%2B9gy%2B%2Ftc5Gql3D2fca9l%2FQiRxz5GHTurP8faGh2MtC6WpMMfevsUPn1NNoNeT5x5KQ6ZcLBojOsQMkm%2B%2BlJETQCiMyyCOLRg7cM6Rc2aZLLdvAVQm0l%2BtL1211BS6YMcLuGhwbSnIJ2MfX670uuzGzU4zhVPdh0QKS3bIfQE4yZ0&X-Amz-Signature=37fce1c6b63f080a29c8448fd855a913296ac6fff9c88497d633a0b8d827b78d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

