---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZPR3YWJD%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T190037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAURoBQpGw947Ch%2FDIoy%2FOWf6dCAm94Ntze4aIs5oHk9AiEAgKU0tsT0aO%2FUO5WeoEx6%2FwtZSfJLxgJGRvz3uSRvj5Yq%2FwMINBAAGgw2Mzc0MjMxODM4MDUiDMjZHQH49ORtRJG8VyrcA7cLEURwiHa2O%2FADBRAWVhyFqNPg25lBCKA5QPvQZX1Iw%2Fm4IAM%2BJWOu4ECekzBkMdx8hLyXGIzy2p3wmemWjO8dFTY%2FBtA3cI9%2BaA0rpsXtsTMegNUqzz4wXwQCWmwKNc%2FaqKuXsZ7FITJ%2BJpYlYn7z8Tn8HWUr4FfXZnLNsOE2jAb%2FSaSI08BIl7YOO7NXEEeJsmElkEbYzru%2FZkrpkSNDWR940jXDMje3ByyoMcrJ3ql8WUVgXrLBkq5WtMb6Ps%2FFMsyk7ghHGDC98GrBALRWkc1sS%2BELNIwzmRsjlaKWbQJmYfkkJJCkl0pkg3y1JoWs5Yb%2FEIa%2F25%2BQJ9ksS8QvmFdYaAp2jlqZvhc18cIflwJh4elXGBVjrOGyO8kVbdSgobS1sM6jKwRnJOYs%2BqLyXkPSO97Xh5VUGgY8IaxgEZJ7rhu8l%2BtMK5C878pUcOmWej%2Fnydt9PaiXuPUxoV78WUfq%2B3rf%2F6fmwJe4lQtGkq6hp9EOuW6UjVOYyrDMQkBDfPrpbnbnBVu02D4YfeBWzYCyiaTJj4W6MCUGYdpUA3Nj9Qg%2BJJQ5JXjJX818Ll%2FBNoNAViYM%2BNGxP33tszcwSv3IlJ2USWb3%2F%2FvKVeYc3WV6%2FLdXqX6O6sW1MP%2Fqr8cGOqUBFOrn3Z7G1jCdC3fE5NiAPUB5AK4JA8G398UEXVxNxlIe90uiimf36HBr%2BlY%2BxdvEmxtt%2Bm02TGoa2wB8IlQYId0fOQXOnx%2F7etV%2Bld9uWnfKONcsnSC%2BkKU4q98tzT9eOPme2dCDB1LCCA9zlMHpk7wXxbjyiyX4B0eOtWg5aYqujdGA0%2FDFBUm4%2F1f8dNoufDoQ%2FKTDaMsfDASYb1yedNgHKwiH&X-Amz-Signature=b6fa3d0d8fc508d184d151c8c80d08cfff20b2a593ae2b2d3f4d6a3a05516a4f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

