---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z2Y24KWV%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T210037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDMaCXVzLXdlc3QtMiJIMEYCIQDp%2Bprxy212ttST0uSE3%2BxHKCP4BGVEd3NblYFCaT%2BNJwIhAMREuwygimf8%2B2hMldmqvQiXO3F8LGU3jZFfRsKcpbChKogECNv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzASGo9whaROlly9GIq3AOpu9ZFLs9TNJTHIbhRyRO%2FFY7kCCUZTpn5qqr8aDMSVLrLkEKcb8uprfz5ufefgy8K63ulqfXUI1YmuP1NPFqqKSQ10zEXN3mHfQTFuzwmKPE3L7IAsLMuc%2F1cAS30eFrGcHUnCfaPhE3ydmp0K0mpYBXFRtWxBbFJX7DMANWGoGO4Q1ABvMXU%2BJyb%2FiRSZKkZWGlJdehJS%2F5B0dDSQ1gdy8YvU52FxT5IvVfMYhIQyyIrsIzQUXuoGl3xygQukaxpypgTjnJomxg5UgYdbrBz453iaJXrfsUrJJonfe3t4jpjJj%2F%2FAqY5jiL35PwIOx3vSsphjSqn5K1iqRu0Ab4fho46fXhFXyamI%2BmHjHdfAHo6HyzbEd906Y%2BWv%2BQAC%2FRXobYA297JdTX%2Bxxy4zSH8KccEIiiA6F%2FBx%2BLRbO727tkHBVEFbYxPHqSh54NhOjAhY5yBBCJ6WYHZ%2B1V4wcQvpH%2BX%2BJ8XhQEvAKqJkc20GEhfRs%2FQYYTffyLzyWZRwOJK3ikg06zrT3pW%2B3Tf2ovtLrjA%2BTINvgnBIkvvqneSpFAmhN1dCUpB5Anqp83UsuLPOZQi2B0ZpS7835vPFZiDManWnxrvEX07tSEmZOeT7a3VgoKdRiq8GXf7UjDk1tTHBjqkAWxglhCfUgra36m4fPoVWTr0gB0V7pF8f%2FIALv2ZbgC0lXGez6lNcbrYsaAU5tUn2TiiKy4CAHteg7vZeEGNrd87ayAONb%2Bp6ko%2BNWzWHvF6CB1e%2FRp7Dg3AghReiAVfVOCJa5VqCoq8DswwwLAJSg%2B82N%2BUFu%2FQ8%2FxgfY%2FpXsa4nDJVvKWKICsqR4y4Q%2Fnv3tcUsHw3Jg2OZMEsHBwmAYVtFe3G&X-Amz-Signature=a81e1b15003e5282b9b33210e86a95812bcab5ccc65efb7f194d00cf2502330a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

