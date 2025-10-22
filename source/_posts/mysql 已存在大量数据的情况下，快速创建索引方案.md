---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SQAZJUM6%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T080106Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG8aCXVzLXdlc3QtMiJHMEUCIQC9DK4AR3Sa21s0UgLT9scUjT16qkDCJl0VlNUB2fpcCAIgPJyTh5tZ0DistjTjt4T8djabIzcrrRKG4pS6y7gQAHgq%2FwMIKBAAGgw2Mzc0MjMxODM4MDUiDO1GFLGkiR74upgHHCrcA5EYPRwOWsJVVP%2B5rmn9cuDWdeiV4X%2Ft%2FpaLiHM0Mmb80bErUsTlG1xmDcc%2B8at9b3dFmjNp%2Fx%2BHLBTgbg2A34uBhFNkjgHHr6lrJTJ1GC41mOXJCOGm1pz%2BDh8kEGLeOtR3dUFxcakDeO2mVKpMUdKw7p9UTDfHcbe5MdvqjgGgbXAgo7qRj0qPinVhhZy1d4%2FULssBVJbKbyGP0W7P5WuuhG6mF%2Bxgyb8A8kgEaAfhBQenLVLZnZEKlmJvBRWWCstHcRexsj3uUlNTwmn0jx1ciBjiR%2BXzQyJOjedI0wX2YHkbLxgXU%2FoGeTWWjYdbjW7GfJpNSwgukgPAxaxpjOueJnXP1WbdBiaBAoirs%2FGNPrwlEG26sqmVFFr2A72nmZBMJzQf8gCuT3bqsCo8VfZWOtKXEnCnzJl1J5D0XOXJXZzuS6kTHkeaygz66erSpdOCrrs29V8fg7EargjMDNssZRg73OfscCF0QQGMqdVuqLLlwCLGALPQOivFu5we%2FCqZ2LzHKcHb7EmVNGYjRbo3Q7gaddwfFwagQLzfeRlnT5m8P2O8NhBUUB88r6ba9pgWrLCug%2FRqhI4BH%2BihRJnbCvmWH6OZbpgLbZA6aoJEWSMjPBsGDfE4niJuMLuD4scGOqUB%2B%2Fh4fuc4paZqgqHNvYOnqlupN9Oe%2FsgS3mUv9zrV55zcSvrn%2Bj%2FX4xrepaUH9KjI1N22bS1iTUA8EBwHE4qBwQe6QWHlLKaF3lxoRoK7fBjk2yLxCJLvJjZVvm3e2A6IK4uTUfQGIdKUnuEm788xo5kjkbUlQGzHDCUh%2FQQsxL2M9h1368DLg3BBBbzh7M1DYVhvNxptIxhzjj0YMBOxSLjfLQzL&X-Amz-Signature=532ce69606fce7ec97547d65eb16f2a738657a9f43ae59920f92f0cfa3b2b2a7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

