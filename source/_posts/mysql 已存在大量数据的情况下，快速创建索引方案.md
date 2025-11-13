---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQXNUKRC%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T210045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAJejGCG0KSP85kTWEpu9o07bS5sdhUPRHSKw1sP7hjKAiB3qCJlYyFUq24ERa5Edi%2BWuUDFaL8Q3DlhNzYndbK5fSr%2FAwhVEAAaDDYzNzQyMzE4MzgwNSIM8KNUBrP8RboYVuo%2BKtwDplDZZxfEHI%2B%2FNzZjuEfgE1TF9QChy5pLzXRn3GB30hlCVfdD8zZba574Ts8Zys5xMp8ndm6UyBWGmaD%2FFtEjR8IAk92yH3tJwkaetZB3hyeREr7a2eW7x60t2cJtqgNtM1E8UFVbfYC1lnWzEpriq%2F2%2B10vlfdVSc5z7DOKA3bnUtXUrC7uu%2FDVXgCvbOnfUouPZKeZqMQ6yrJ58gtoyLuVkcAJ4RtWW4ecylhCuAzoHLZCYp%2BCgl3J3t2T35F%2FNt60VxWevpXuKsHwJkdyeuHm%2Bk%2BEnnvgpHpdg43vy44SLhgIGERQ8%2FWnaMrbvqX%2Fusn4nagNatsg3h6thx0rAHIwvVRjFQg8IDfQV8nlTyzuzs25JIX7OoeXlx6AhdqzctgIblaOMAFjNMYWvx1qYuvAKtMZU7NXZCzVldpyZCTmCNPV0KtsnOiVB0y4l6JEbJlHctLEAccwwtZMamhoz82UG1GCJB5Lk7GECEWM5zJt7T6LFZ9eeVTbdYnZ7fIqV4YRSl0IIVFlOuQUCoYHhQK0IycUrXdPNud9fBmx1NgpVOoeyk3%2B1ggctt0k6EVeNrbQIFcKgVJOXhH9T08Yxanq0EG7fB8h%2FuKDeegW1xyzODFqej9MJ3lhgTwQw7PDYyAY6pgGhtt%2BRcwjHT0JoYWFySzIvdijWSJ%2FCE%2FKkXk63kC8A7Ml9OQQhwX5JL%2BX%2B9UDdY7u4KVIndU5JI%2F3J0V%2F3LIMiW9ThcArhZ15UY5wo%2Bm2kQ7I%2BJCUosIS7S4Jo%2B6RMVKMdTHYzqMJj6br%2FF6RfWTNJDKrtESRp9p121nduEANsWgmkAD%2FR3jXDVKxUms3%2B8jrbeGTcXO0M3oDWVIVg26G2phq32rWq&X-Amz-Signature=b7fea2c5b817330d309f4fe2ba2be79d60b61e15e979981eab6b5ea641d48601&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

