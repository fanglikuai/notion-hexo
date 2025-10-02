---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665ISGLUJP%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T140113Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCID3X7ZpbSwj%2BjGZw8l3U0iTKGr969CkSDHzM8DXxnH3KAiEA5LbQW0tD%2BCIK80m7ko%2F%2FMtrwHmPWox%2BGgtS74NQrCIYq%2FwMILRAAGgw2Mzc0MjMxODM4MDUiDJJbu2xFSnnsIP9RDyrcAzrh4%2FynrQ%2B%2BciKneLH8nY0UGpO4zt2XgnsC2RodPDf6SHb%2BGjsffk5bXVQZoXsW85KjAMI4CD8ofM5YyvM6dfYvgX4obH1QdG2uJ0ElsXWQaZIGUH%2B%2BII34Nv8egQ%2FWHUKjP6kDBYOTwavTxKpEv7jWoviySIJDFQJjT2QoDmW3dun85pewlpF5Sd4iQtAQWaJQ4QeiDmDvshGFYDQspSyCDRnciwx2VYdGnLPC5AaQts6ZX2bmdKV6gmKPIKt7vI0%2B0BEnAAnFy6OJzGhpjvswMct7XFvmHkxYysbOeXhxzVHzRNJBQFGmztjsj6JK9%2FvfdfYynSIFix0WIa1U7zWrqNsO0mi8YlpQYjowfmEmLzFIAN%2FUpWA4YXeKcm3GzE7zfzS2DBt5W9uOuG0g5FxaDWffP4LgCJluQiWHp0HfmRfkwAo8WojB5r9ZrfDPvNeJ3gEZtpB%2FSQ1iDQDQE74BEjZe%2FJaZTIRkPxrTq25udejKZYsiAgv4dEkslZbEqO0Xdv3a1lNesyqTjZ8veKxanKQXqgqnAxvbvkp0xatMVdXX8ag3S%2FyYTVpWHiDfH1mvTkQZ0HIhYO8y5cg4hJ6%2F0ewwkqN%2FOava1GUXbhMCAqxtxoG6D7w0bWtYMMvD%2BcYGOqUBUbaGqVcms8uuIwBPRYU8ylvegGPy0%2BgwunQh3LAROWx0s63egx2Baxdci6ZiMaryfNVGVhIgpFj5Mfn0vt3wTjLqalbSs0AK9X4mVlgggGNCF7E2tYoN71c%2F1sfCKK8JwEIsHt8HYRiwDktpwZMNXxe5reNmRF0B45Ej9FQGfPhBiqcA5593E9h%2F9A1B1GCDaAlst5w3zEIR53GFoBEkDNoDwvug&X-Amz-Signature=16186efa09762d44dde550e58899ad2cbd82d7a454b0e40f9d11d2a1fcaecc59&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

