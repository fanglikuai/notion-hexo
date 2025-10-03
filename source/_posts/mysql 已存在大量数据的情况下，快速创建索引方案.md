---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XKGNCVB3%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T080046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIH48UClHIk9o6uLn9aftRK5WVaiicLx37Czph7UWKZ%2FAAiBMgnMBiZo0MTgLWMNqZ7yj0qMYBHbnI22EKgOeGBi29yr%2FAwg%2BEAAaDDYzNzQyMzE4MzgwNSIM%2BziwFahvKvXSMJqdKtwD%2BMRl8iCT%2FIvOhP9yaGMiS5wXvoabG31TYAkSwHTw7PsQycb97mUbMGmJdQkBXcRdfjZOPvntTL1vTXEgx08j1aMG5%2B9bx94YA55HLWOsvX6Xe6gu1x12eDyisbFfqJ23tikr1NCrE6lBLb0m31BV5zKJM8gFwz2Bxy1XORhz8yqrQYz4%2FGNs4%2F2UYvBQqsnqR3K13mXyggZyZE6AHBt67g0zR5z70Aq3hP2C1XAdvM7ee4Ee8l7Fn2uBeVAVFb9js7L1ATWbKsxjJWOynSsuzjW42HCyWmiRg7BErLTVipYmqUg89qe8YitvG1L9TnfjiytDy7q5Pp0enyS7m%2BtWi1X7j%2B7OUb0iDnJu42m8jF2Cvj64ehBef8aPwUgbBdp%2Fa%2BTG0Zbb8XvR8rrFkdH%2BYPWLorZoQGxtgPfJEeN5yUnbPKD6pc8n1XiEGF6cBJb89sKlxPWd0HhyzrmqsWNDBBlE%2FQAJy60i6O3KOAyrAm5%2FAfaCH5hYlxJ0y%2F68%2BEKsgijqP1Dne0JtV0HVN6mKVgJWRrWgNqP4br0OU%2BTbW3DccwzUqi7sgghPOANb0Bpi7AzfaXQAz%2FtDg%2Bq4L0V%2Fh9IJ2NHJN5QPwdTsz3C7OwWDqjD67bEUWL8hcA0wiK39xgY6pgFEEiSpmz%2F03yddonFMt2tl0n%2B4%2FhrdXRb5NhqsEWVmGhaAuwzFV7OshlIyovzyOSMsteLT%2F2HSUkzz84VLXJzbPvV1AooW0KQW7KY0bPBE6THpf5F7Rg3KzQ2GyvT8F14gwPlv%2Fpwuul48ywgs0hN03JIPZwG%2B13rj4ZIZp1C2tRNi%2B5yZ2vLbD5MPlkROm%2FY3taqC0%2FmE7mKtGbIh1bv%2FeclI3tMi&X-Amz-Signature=ecaa3a0acb05f8f3d005fa567df0a9025da1e05e9f78f0d839efebc9f27807fc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

