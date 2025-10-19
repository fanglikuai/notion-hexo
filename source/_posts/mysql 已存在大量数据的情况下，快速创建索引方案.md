---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667DVWLUJJ%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T120042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaCXVzLXdlc3QtMiJGMEQCIFMwbVJzZ2mV4hM90SglyQre6CUxlhpMlkUvEo%2B7gH9MAiAN8dF%2Bv6UwRyjVnoGeL4593I2AYJY7Ksi%2BBvuyOwE%2BgyqIBAjP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMWTf4vfLNUNPqSmdkKtwDFgjG2iLPi%2Bj%2FgX2ltDYUkTE%2BCWRjmlg46nfVmQtOQSJOtKSKdJTTYxU883xCD%2Bg%2BaSJO4jBjEhFbNMqkqirZ4qRrAiZRWLNgi6Em0IJvsoQ%2BnsEclndARJOeEvJXcqLRTwEPrR%2B2VDCmRjJLg5%2FDVz58oub2YN9VOdjstmVzoVRriKt9K8EY21NtLKzHCKfd7%2Bk%2FX1LwVdXnCWKi7jzyer3DWEC%2BMUjZomtCtN9NGTqIhEOg1kdpKZ1nS5X9opESFkNwOscchOOelVL0i2YHyeeRQROT5yz%2FjnS4PhNhCcPPfSSuCZS3lIfXmvkaWoXlVQDfaEzKS6Fn47xdwhUdECJvCvDaAd4a%2BR2h63HXAjYJPPMzRdCXDIlMS2AGP7tJYGeveyeXbVaNONAhMqLvR12RQPRwxf7NZWx7FfrhuRPO45IcCSBPKLeaCISHUU4RTbmoQeaS0d%2BPYzFk0UQ2eslQA1hqr9bxXG11SwqH3dwCKM%2FLVetq02q5suL2WctaQFSvMw9Iloj4%2BisIb18BjhJjnLiXe%2BaW7oUmYcN3HP1lOgE8dl99P6uBsa6qHq4gSqDcdgYHBOy5tm8FnuW5Yl9AegdNeTwH51nLaH%2FvAsmYE0Pj%2BPFpjZef844wr4XSxwY6pgH3B3cZPJKNDMl5qM2xQ%2FBPzHS3RpYR%2F7BjaY80fNmA%2B2Xg7KO0jLGu%2FnuH7gmRHusIPc4nf8q1Jcxc3IcKezVHGAn3JgCbkFo2gyhDa%2Fh8P46QlxDso%2FPEazuhNLAtvxAt%2BV1fRKiF2b1aMhZZedC1FgukqKO54O7cOWEabCqwQHhWmXyKAJwL7pdxSYXwvba474ZELe6uw0v7%2BOu4bpKSr5qUbjQk&X-Amz-Signature=e51bc4862891d6a609101ea7ab4ba29ec939e81ee34ad49ee2900c7e39d26c5e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

