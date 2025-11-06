---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q5HT43MK%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T080045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCbN%2BM9T30f4dq26Z0w04RGUYapALhpuw4DXBIbs1l2WwIgNN9Qq%2FJBGeL%2BYouGt0S3CMomrACsr7yh5qKUJHagLT0qiAQIof%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDB%2FH9ZDzndukDCfosyrcA2aVfoZsLzagkr2PTQpRfwP4aTutJXSPQ%2FYq0rGaFRHGvBaP2B6RhZzakM9jRRanEu%2FoLJGe0QKlx4uMcNktmu0xEsocIqIVKlr65ZMEzHDDj3qywJiJfYh9TFTTUWYvzJFw4XMO2uDPLwKxRUhF%2FSflfiCkXkcwjS1AkD4GFTeIRwoSQml1MuqmXEl%2FOGx6iOot8vcvcumo8r5UHFpOacPo9%2BHMmTbFdCy720pHkgp7EPEh29hPJDjqSpuHJLIjomcBAOYISuODobv1l3BDAM1OLtqyXADSQ4%2F%2B7yfWjvXpExRU%2BABavcG23qPHPJc3mYHX%2FrSw8XWhOWJZOShMx5kUxdUquNLOYMvVayzlbj7bQixLyK9IpZQh4fwq%2F0PA2gcOJUgM2KfII2Fzwe9NL27NtljLZi0YMCCx8a5K1sKWn%2BbfcvLF%2F0B9UE03aVFhUdtQjRJSsUSw6xXwL6klBVOogtnKNvJow9ct%2FGL09HhlcMhwIlS4NT9yGLWZFhG9cled2y5NUGMhisZ4UP9YqLvabaZFxFkjk3Y7%2BIO1I2zdbo3bwfIswT%2BIcpbYmKRhbwgq%2FKFx8pFgNkwj4%2F9eeYu6b8oxFa5jYrl4sacrXaMKKtprnRt46NAlNo%2F1MJ2gscgGOqUB93xLuemZOypfxfGJT93eG0rwqVS1aAedTgAp6c%2FZHFkTKXXogv%2F2y%2B0POTaVegm%2F373XDFMdcXWXsXiK2LeJ2Qhp9PDAUzYDsz8iNHl3Sn%2B0DEgHfGJQSGz3jVDsFx0lSz6NrLDo06e5qHI1VEhu%2BqVgm2oUS1GnVLkALzecJRLGd769dfrz9PQJmR5OAxeYYQ7tVgXS3pEh5VrxBbriNtdAhVtp&X-Amz-Signature=7635d8bc7f747ed408f00bd1de83b63f7d5d1a03a19fc0eb185ffc50b2595eec&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

