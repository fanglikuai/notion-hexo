---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YP2BOGOM%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T100046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJIMEYCIQCr2hrq78unOVCMbKQXtdrf31E4z%2FF9lWoJZPp1%2B0rv6QIhALqOU%2Bzrfj7ZYU4Z6iYg%2FksDRWRHawZVuf3nC67w2qIkKv8DCCoQABoMNjM3NDIzMTgzODA1IgwoC4sOuaC2tulSHBAq3APnZmuCdgGu4a7UBWq4EODtHeYew4IG7qhjowOwU9QKQggKA%2F2Q2JT1f0rf2QPtDeayE8bW4mv%2FuFy%2FSyVxBfgQ3DL8qkO8mjXlNpuUqSAxVMl3ltbPOSDMQpprDEszfv4%2BCIctRa8FvPVD9wJqFi6yDJDunRQe223XBlUTm8R0ww1QDRI262b2Zg7CLknuz2O6Gk%2F4r7dw%2Br2%2FEx9zW3YyxOFvIMPvmcQB7KTfPn8C%2FTikqmfiORjtjfooHmAql9AiWWrvYi4v0d1Z9Oh1dwxh%2Bk7rX%2FXaXXnEJvuzUAT7Cqr1EfpiahBQMgp620Vplfu%2BmpVOGE0qcDkJOnGBPOMN3tCDSlAE48nPmRbpq1x2B1Xb%2BBRh50xhR7u4yYW3WQP%2FDT1Au0JwTD%2F6WO7KcmivE7Law%2BFj3SG%2FCHYxnn0WE%2B323JTSMbNzPX37giJYTIwB16yti22Ttr8SM%2BCe%2FcRB4DgiCo6ixgtQyAQwCg8aD3sG4sABab7NRCfQwlpR8Ipoi%2FiIvoz2YOc4eO%2FqVBnESCp%2FXZagIpUWj%2BTiIdIwB3eXkBoOxt1Gv%2BALu0WRbpnY5K1aZOaUXw5xM77x7585HSeTacxRZkDP%2F%2F5QXPF16d7Y5UXswRqYJ51L8jDauOLHBjqkATYLNiHqRyWoCO%2F%2F8VfwgwTiaNPCpVlNAIIwtXPx7WH0Yr92XEEpUrV9XdoKxO2G6C6B6dZ5Pk4FXD6a%2B5k2YjiCHBwvpc7k1F08zkBuAYRm74rwIGdMkmaYB84T%2BUiHHKdgb2GfMf5k4%2BgDUIWK7NOvM5q4GuRkfpmE10zaaWOekURY%2FBkc51IxGnRwN8hrd3bTn4JjMHAs07OZ9CHAKrJdyQAR&X-Amz-Signature=bd4ce5471458fe6cc929b399c149e50b9aa48a54e2ac7e00283594723b782bb9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

