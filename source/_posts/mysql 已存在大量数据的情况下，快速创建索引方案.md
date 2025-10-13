---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662DRPQECC%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T210048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDMA%2FC17zXB9zsFEcmPIij8eiZSiU49PCZu36v6vvNCdAiAc%2BGbETqIHJcQZ8MqNSAVwu2jHzjmq%2BK%2Bc9MDntvfcFSr%2FAwhNEAAaDDYzNzQyMzE4MzgwNSIMQTHzZv1jVM4MYX3HKtwDf2NR7fXk4sngC96224Qwa6ZLCBfUKK02KDlkZi7qHnvRdXZWa%2BLs7P40JJ7lbj8yNWf%2F0RoZilcFkh13a6Tpp1xcxM%2Biv52%2BHCFkCakkLzOuE8GajzKAYyMaDO4sgEWosd%2Feg0YBa6B15AqQ6xnlqpnqawAeYe7x5R5Z%2BMfDToegOR5QNTICQwOIiOxcBMWwI%2FIhZ286gQmWrWqZrn%2Biv82PwExvVTUnwKMAVS4VtxmVIgUpoTO6UGwyYl7bjFFFQWHsS0MPI3hedxyeMPugTyxvMf7wDDfX%2FeYo%2FEI3P5m8heu3w5mQrq32RbmHDy1yNmyKZOoHMpENnPzKB7f8O2u0FKwHquQ5dGJBr%2BYNYQmMAx%2F08gewMchUkyw765A8j8%2Fh%2Bc1Qs5sZ0ODATb61cw%2BqNaMSR%2FzVXkmnSSmK01pFTz%2Fv2nBm5ZPBN1yu7FwLCOUZtvUb6OtT5qfmb8noRBqfXIKRH6brcgfvISWdFrkvlleIgPfKo6WFSD8ieEfuWoTJOBNiDj89VB0Xf0QSMbeY8C2ZDx%2B%2BBHBGLQtUzyXvMjCllFWef7wdXnULbXKugQ7eaibjbiR6xOnu8WO%2Fcednb4b%2F86zuKjGpAAzYTKOcOUHmiTf8HASV2G8w2rO1xwY6pgE1E6a3VjTE9fOfZ8zJlD6cUUcw%2FVdH6u2vbnS2iWEC6yRwrn9ayaPJ0AP3akgEHRM3akIGmSuKSga%2FA5%2B8ZGpOilnHMc2yQP%2BkGyECvCsvswz%2BZASuEBPuA6RuFduNJx6zQQdoA3S8ls7sQmz2TpEpl8FkzgVnjQOHN7GA3WS4957T4TXMspoRuO7DGv4Y7qMEzpbJa5%2FacoApQ6wjMro775d7naA2&X-Amz-Signature=981dd31d2d8077f999a8253c20e6cc0c9baaa959f728ec2f9a8d27b0f4f562fe&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

