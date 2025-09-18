---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XQJENITT%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T110037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED4aCXVzLXdlc3QtMiJIMEYCIQDEWsSGAwt0VhuI6RwzGzRRvWJwmPxJCzNsz5opdn3lTwIhAP0yHzWQbFhqTtEtvyMKXLi2g0wrHwmCg2Z0IKVzOTqPKogECLf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzZsUensOEp%2B9BCZL0q3AP%2F91abowWy88GGQNKXI3T7ywHeIjCfEYIHYeIH92B7ynXzC4aILntZWvyQNc9K4G7q6XcWy1cfDE43MXclxIrF6ja0rkUOtim5esqzVjaYyvT5YL6tNKXS%2B8tjiewyfP8F9iKgsjjSZ7mAfWNjmzZQRMZWG2FlD3YtiiWe4O7%2F1mUUC1kDj%2FoR1mcqiWzUXEmaMCGWHR79N0pEA12Emp0Pmkd3q4qeNTcyaQ7OVjVinVq2e4Brsaef7GOYXI2tkPqxIOyKvFnVPf45r%2FDETon9QXAZi8z6OlomF2BHRwOHyzQDs1avxJsg74BdrC4AFoz5nQwyuHql1k0mkHUYqEwQJeUoxEQlhmHdhKZzY3y7ABbl1PGKDj6NR40aoVg4pH0w6o4nEIf6M2R%2FSBCY8zj%2FMsIA0UqD2zOg91CwDUgqUcGG0BqsrdyBD5q9gnlTVUX4Qr7X0fRdqtEooygvJgXvQpT0aZf8jeSsR6MYkOKFwun6Y176kFk3yX%2FF58X0XckFKLus6e1AUdt0U%2BTShuYEKhtjI2JFTB%2B00nZWdz2%2BpJhDGL8gPu8YTK7uVsxFWmzjGEgYGHxPFdzGIAKMFsGhU5tjeniewOOsv0fkKydqNfEDWK0sexTIvFNRMzCgua7GBjqkAbeq0ErZyU32Pqo50uvoAWdXKE9t0XyJTO4Pezyr7frfgLGEYlUfITbb%2FOujgWZ2W7kA3zMGGkW1Yd5Yo8%2Fjx66jXJHUZWyV%2FWkiEsrDy%2BQdWXWGirlGFZFNS48JXKD0mydy1iMRtjG%2FsEtLi1GCFt%2BUXEcdPHvmoeH4hf6ZSMXhh9bg7wkqW4ZD53NdOMQEEDWmkezyC3ine0N7Ii8cj3N%2FYIr%2F&X-Amz-Signature=075171d9d7b3546211d20aaff0867447f114adec12787ffa2ea14091b6a09130&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

