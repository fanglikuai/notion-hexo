---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SO764X5E%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T220036Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGUaCXVzLXdlc3QtMiJIMEYCIQDd846ZVj%2B8ZjwEp0MIymXJ5Adn2PLmaFNX6eA7AcuB7AIhAJ6T0zmQtJYSpCb8apgn5Eztcd72Ca2tGiyN40GoqgKHKv8DCC4QABoMNjM3NDIzMTgzODA1Igz42e2YeLPHWTWuugIq3ANASOxOgnteyO4IpgI0ImAmlcv2Sd92LK3bnZm%2FfDRzpbVwzEdqyG9z19ERoYJoyVbRkqsRqQITV%2F445ojtdm4KuCgTJQMJ57%2FKGM0eZLbGBUm33DotZrir820Ca3AuFXcFOSwN0YQ9Ygdhh1JAUonJcrK0xC6DoALn1ZV9UKyShbq2ClNjCpQchQkAp8KTKoXWC3e3nTAOhXqlU60xckS6raFPgIFX4XWlYYvS3GD%2BTWPLEQeKPoum0nzCexBJSUeUa0%2BBtMHaHpNHMcCNsdm%2FmljVZqBENFE1euZWReK0VkNwoNKE%2BiMH4TTyj3z6AL74bfcRH3ZyVwovsl6Y%2BukX%2BhuXSXPzxOPLWvnTI2w0cFvKtbf3c0818roMTnhLWQ5LLwyI4O3%2FMl%2FPfWeD4P8TyUS4E4W%2FCwMNSNN1HDD9NDx9yBh9zcDiwFd0ncmIzVQoGihl9vxYYZs35HisM6ozpRqiuSw0uWd9wQCRaWCJGhExo3yyX9r%2FK%2BV%2F1OK5B3MDvPmcZassgsACyopLu1DioxWKc5lsMcoVMQQGXqOaNWIBYaeyKCtlAEhVRXYg2S3mng94F4VNjsUaQ%2Fim6cMWz%2BSpvwilJLOmRt6DKgLkX31YHBLdUZOGx5qBhjCUxojJBjqkAcG8coDXYO5Zr1E2XKk2bKnyjbNYusN1MdQEulHDPlPkwqJNN40hdaqXqag6PT%2FI6kyLlTkK0PgHBrpiW%2BPHxve1UASqchsbdNN5q2QeSRyA1vcwj2yOo00nBJbmlh%2FMo6PDtFdozblYDHoeHn%2B2c%2BQ%2BT9sYb9ph73UUhy%2FZipm4MbEL99mCYa5LBcTX2nf2MZdnfXmWWTkLwJzCKfLEtPYTNie7&X-Amz-Signature=d8ee6dee7e8d97e7ef3d63043dcedaea3ea6153fdde1f98b43be16aa23300ab7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

