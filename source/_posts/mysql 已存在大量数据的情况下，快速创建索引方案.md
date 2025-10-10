---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663KJU7KDP%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T130057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFQaCXVzLXdlc3QtMiJGMEQCICURU%2BtS7MA6c3ki7eFwWWs9EHa9jQnv8hUZE3a7jsqsAiBmNMhfe2s%2BuANCvUH2dz4LPHiu1dmjg1ASJ4kCmsyhxyqIBAjt%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMhwW4LoZTQIHYFwklKtwDvWstsU3G67zFA3q%2Bh7TZGQmyqsocg2oiT9PzSuK2ynOLI8cVgyJKQO%2BOksDFlSdfbnLBLzak8uhxdAu%2FVTCiTbw5ZkXfo9hLJ8PuPjkiPmKRrjhUo%2Bjc8p1QZjmyBT6f1Il6eCLyEMbuP9LCG%2BdMCXxTDKl9uqBlaaTZaVGN1IIlq9HGec5G4hlkYkDE116CjI2kVlIY60l5te64B%2Bk36IBPsXRNvC0g9jwTCAHWldmf%2BKC8tyq9FA89JFb%2FAlhimaV43%2FbtT1misdzOI7ePIeT5R1blP0QaPg%2FBRFttcgZ4j%2FtFUeH%2FX%2Bw0UmyAK95qnZvswqFZHT8DpdYHPkSph4yeQRxPDsuFdVMJx2BqU4zSK70iCrlUYS4RaZbn%2FNP4TW8WraXTaza5Ds7v%2Fq%2BiTO4CCtallRuOBaVPrhor4qiyCdL7SIwDS594gRLGCN0Y7%2FSDUKHTyCLjrUXyWuuwtJeQJk574tlT%2BtxIHFoK%2FozptsL4NqkC8QbdVXp8zjPGvFqwKP67wT%2Fwn7cu9Ay7TM0tn9aEL1jWyyGWMBvBirZM7GTDdKZIm9l2NBkJb023RSm2yH0HXnFeuKhToP7Speom6k2YNeiVD3fQY4OJHdY%2BFpH9ghZYK%2FgOC%2Bow%2FeWjxwY6pgGDCBuPM1RyNE0Co58zgJiT771rDYzXKSqAv7mVgUrJ9YzFvFE1CjecDQGpH4Sb4un8oWdguF8b08ylaqFzbmtJb1S97SDlnI3yKVD59C6uzgS47L4mra%2BgT0Ase4hOhVytJq8XeP3blYj%2Bz1qwGUmfNn7OfGpwU8UYiNKPY9qsno9%2F3F9jyMBaLe1z%2FZ6wuZqERWkcwCjlKEMX3THYbmq2p6%2F74O4G&X-Amz-Signature=83ca5469409da5bc8c71b68ac31e06a9496dac7431dbf542b1c1a246c9f0a1d0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

