---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46636CYFHIR%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T150052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCQlzIONxeeL6udL%2FQ%2F4q2cFLp8FnW4oG1I7hpb7vtKpQIhAPyq5Z9bUkcPHW1yr5hCieMaAv%2BiTwTF%2BOJ1irvyGsRbKv8DCEcQABoMNjM3NDIzMTgzODA1IgwayVUzxmy0PURuM6Eq3APWW%2FPbKuodvAKelLzQYSWIGCl%2BxfLWVYMwjH9omJSUQ00461OPfbAvxWXZBJQ6Sz4buy%2BCdshMfQJAFpCKtyGWXtIESPr2AIyq7tE%2B2eCPFixt202ARvOgjTnWr44PHPxjkXjWHXepiHTmRe4TGOwo0McMM5YFNi4wUHUsme8%2ByYGqUvk01idPZcEQXZ4MXQy6stFruJayGnMu8zdrDps25COSU1zYBNEhYo0ONe6M0t2kKBTfR73FfZzYesg7ttbSg2iyoqCaPbL39Q%2Bc%2B3athURA2ndElZEEn9Qo5c%2BJWU3hIV8kP7yrbpqZ%2B0NnMlkrzjKEwdJ5atrwrl06e45oE1TIukOG3XNqbmo3biJMD8zwK4Ud49BlXa%2B%2Fh7d%2FZTGaFDwLH%2BKkmFGsWYSSCf2rzQOqcQ4YGJ9amhQoRM1jN1P4PSIfABDvsPCJNtTpVm%2BQRHf%2FC5BReHG1SPNDIVB5yWp0vJdxj5luri4AGBrZKxhSgSMFJ2KknDR1afrwJoE70r7daVI%2BEpFWi%2B71rcRXoSVnb5Ue4XcZCqAGe04WLowAf6vKDxqrSF0uNws49vBKhVjxIwNw3TFDwkcd8GY7dvf7Feo2F8nXAvTZEgf0DbKY5XVwVqYcZX6OAzDD1srGBjqkAf1B%2FiqQ2Ct3CrDkoksGUDy6t5ZbqIersuoQyqazaxSgpipEg8fqqIZbjOH34loGP6SMX8cEQm1NPERprJv2cOJQqQDbR2Kv3nRf8JjlkMQENhW3%2FHuKMBzqUnvJ6SeJV2aAKf5RViBdQxNw6B0Aijah2HAuZaa8cKKbjFTOQzUwTLtL97gdhCKKZZtQGeDXUpAd4uDnSkSL5igjNtqEzI8QjuD0&X-Amz-Signature=9755e2f69f5f4d03cae4bcdb29e14964a080ab31c3ab4c8460947d5a50a61a9b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

