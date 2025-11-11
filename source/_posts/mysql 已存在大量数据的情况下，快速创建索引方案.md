---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RLVVAZ7P%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T150053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFcaCXVzLXdlc3QtMiJHMEUCIHONxqwDGG3t39g7rUUdilER1F3QnKB8x%2FC0dzhJW0OgAiEAsc88BM0Iqa9H0uvWmGihcu0bxCq5f72V7hKegPD%2BreMq%2FwMIHxAAGgw2Mzc0MjMxODM4MDUiDPJDxbQkdqMasvSUjCrcAxsKyynOp2oGGMtigU78bd5hbXrZzE%2FK8MsOuFTH3Q1077ZM3emzi7pjAv5QgQS5mdVGfkQzciX3c2tARI4P4aTYUlzyRt%2FTbN9KTzGES0BuHLWFFi7v1fkz2sV%2BsZ8MDvdErFHH3i4YCDs3TlNK%2FIkT0eM%2Fw2fdeJISkyR0N4EzN%2FlYKbd%2BxfIgJ2xJZyZPERwrLJssN%2BtfLEwu%2BqJgkjVcnJ8VGA%2Bq1C1vqiIZNNiS3PP8Glhl7ZhtENWSUj5Dd3WkX5UOyy1S2JAr8dRJSAVEPRmQ6fOUi2cIzuPpqUUpb0XAQzYIK%2FT%2Fu0%2FVRXWNS%2BnTVo9xS1UBr8JLkFdoO0KmPRVq35y6f%2FwwL4mcDjnhAbXX7tYy%2FMAe7qTw6lR%2F8eIiPfrz46iPql8DeV73zEGVkD%2FaqcbiYDMGqmuyqej%2FAe9Sv5Xe0Z3AdT558733rQrOUN3quk%2Ffi5B6WctC8NYGLdGACwv0H49doeVrtW02160JeHds4gMQVqR5jiVaNVx%2BZH8c1syslJQwTr%2Fe4Qduj0n5g1MtwbYr7FDE8HO5Pe5OxZGFB6CVt19Bt5aB7MGeIjny0CeNwywKcQEPjAfSGA0FqdXyow34XAGLVKMZ%2BPo%2BkwJkfO33WN9UMLONzcgGOqUB6GPyPT0CZkHr2sFeYpJQPApdY0IszymMeB2g8%2BPYxUK%2F0glBO2njmm6bMdZhpbppJ4boMoTEa3m6Jm0fO%2B0yBRFT%2F%2FCxx5TU2qnCHOqkJlYCg8Zeg%2BovCHUrRJIG%2Bus2KUsG26hOovFplnLHErw36FQD%2BgqaSfnEeDG8w8iYAQTJNVGEzMF1zWVW8KUOwMEycq0yG2pWtTCvo5e5v87riAfxeGIr&X-Amz-Signature=60f65e51ef6e72fcded3a4cb5dffb6bfd1b79ec477d6c734c103b4e5cdd228f3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

