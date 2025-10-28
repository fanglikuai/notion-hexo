---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RBFQGVBE%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T150102Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAcaCXVzLXdlc3QtMiJHMEUCIDXujdPkdnhJJfuIu9Dsw7IJz8zASSKXrsLiLm1%2FAUFVAiEAry5rfBut%2BHExez8IiK2rwJI9nZqGGaZelzFYhIWWDrUqiAQIwP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFWBVpQYnsNMfaYpRyrcA5yLoTp3ITEl09mZhdlhpfKNu02uJ7jtzBIZlB5wpl7UxB5U0Qsy7kND%2FRZiM0dp%2BnRD9p%2B6UfjkQU6ObVgNmZ%2FjEM0sxHaiIvNlKtNVsKkSfYskcKGMADuQAK6bxF0h8so0Tzx5fKdP4gLDIWT1EIWmPDdb%2Fd6i8lnoCDMSYKeBsDeEM0voEoJqZcDtVDMKocBAoBVjTWkGmjHtW9glZ2JFG2voHjRywGDjqwz0TxzZo9d6Hrf0Y6KMd4WrGvUOFr863E6NlWe4ZOiKWty5DrqJHvUJj3RNUfFWNBxEGsY17QvqNoPv%2BlVuxGukzlf6LO%2BsDlWRR2Xx%2BOHIpyoypg9AhXYeqTgvQKcqD7Tms7rMXofXYxjK1Mj5Id7wij1fKZCJyubXJFSKS7iN3ygDenoAR4GBE2hLQUD780gBZ9ChOcOqizqN2XfD%2BzE5F7G2IK5wLUQpUx6oPDvBYfWHdGq9vnezn1DLNhB8CC0Gzgfy9hwpMzF6hYjIvC5aeLsMJl4wYhK18F4xq3g5LGxM%2BaAcEmuHbR2OWXRQc5eZOb1UCvvs5R3B84p7wQM4OEJZFHz7tbUGx3wemF4pFUu5Qbk0%2FQ3wKKFuIcpOkueRWLouCDdl%2B%2BhhKF%2FXxAzYMN%2Bug8gGOqUBGgvxtUkYjLjSgE9GuVz6GGKWDuYLMTnMemCrk6bb6bFxdug%2F18%2Fyet2PoGfHmLIjCAnUfwTxTjfhq%2BfXFwIcBJZ8KJ1GLAD5ZP8AOZYQ94rmKfHmSZdzfumJ8yXyASo3Uh9ModPFwYuJDSr23gIgeHX3DsWYi4mWJDrcsrnDQAF45dw6AcjYFXbld3ogtOffXp20C2SCD3H4AVMUfMliwFzQOeCa&X-Amz-Signature=467f8a35b17d32e9b8c186937f78da9bc35438c782ea4c7b5a9d458d554e9b18&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

