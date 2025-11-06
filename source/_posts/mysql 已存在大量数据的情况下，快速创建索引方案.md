---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZFZJJXD2%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T100040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDFWXza5ndkioXJ4DG7l2v5XSB4Tr%2FAzQDPWzkZPgi1cQIhANnEPKCANEv0UtJ6fbM4t4StOtUrZkM1wlKfD99BWy1DKogECKL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igx5igE7wqLrr8I4JMUq3AOFGRpXua%2Bn%2FqzwSLjmBujwAEix%2FWC3MAeBhUNOHw5n1riiVzqGlhxoRNIn7nqfCaNDkylzlz9KC8h7CRFk%2F3%2Fuqzh7yM9A%2FQ1%2BbjYrftIoduYeV%2FYsWQJZW5e09Id%2BEMN6Nf0%2F7gieICKEdFWOYJkzry0oXkYRgzVgDmhkEEDuNNcFDAeAYZ0OT1wcTb%2BK%2FQeOiHp3vMVs9KfWVqXCdaD2iwfHfcosMMhSmwxJ%2F2aag5Z1isVTxxNI2StlEN8HxUzAMbtrFNsLNb5N8CXKadyI%2Fjb%2BinsEyZJkItuB0C6uSekyzI56C3KrZuK0eHGnNWiDsVWBE%2BVj%2FmWVU6%2BR96IX%2Fk8pIuEA7QtG8hFHAr1pdUJhWgiASTHJ19vcHT3LQtjFtQLy3n40n2zVn6DPyhUOCAfd4W6wq67ty%2Fr7aF9mxD8WzY8Xmd4vUAmYnJG6%2FyHP8PvlMh0F4fQnKXjwPf6A%2BN7FO%2BIfZW8iHHP1V5sFe2cOv8EzeCSqatTLTqK0oFbf9pED%2Fi64L6SV4KeD4qZL3wdqNmgF3a6%2B4mbSxrgDnhkvYgfMEQwX5PWYMrzJZjJ%2FdNfURRNGNOlVrsXiHlMqAbtjTkOIm1YQmF5sEJhdx26d4u8OPgturlUgADCTwrHIBjqkAe0M8BKxJElJ%2BFkhT8DNcQPip1KlDLSy41ru%2Fnc1yb2fOZEZKtKtOYxOZRq9zA8OEBhBqpO1yWqVh79SeAwBwzTFAfIjxpLbsVALiQ1%2F4881g3gzzrQK5zjykQt7XCKUxSrrbdMkhF5h0%2FztROA5UeUmw%2B9JlX6nwX80DiGyMuWrWW8xDJxt9Dl%2B%2Bn0Dc3K4idK2Q27XqFKN5dSGcA8Xo6N%2BMMej&X-Amz-Signature=29698d0f6301851da0aa1cc3a85e98efbbfe086a226f747a1f0555733f3246ad&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

