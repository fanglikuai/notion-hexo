---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YVE3HNC3%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T210045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAUaCXVzLXdlc3QtMiJIMEYCIQDCjMaw5ZJ5VhR%2FMvNhrIcttAs%2BGBpbNxyJu0%2BklS8XxgIhAKrSSqxSgxMumK6T4BtFJr6CxqkoU%2FpYTu6e%2BeegU%2F9rKogECK7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxywfZ8hpVNhDFjnhwq3AMBcHPXxAaMPmLrUVqg8wbit4%2F5e7A%2BrEf2wMmD9CmnmpWQvjoZcj%2BzJyE%2FNG50vZWYrSY%2BRsb3wf9wNV8vWuPVTCcyjuQtnHhocKIopKkuWviDBxvUyB%2B3jYbmWfh5tw3spBQeuOV7H6uvzx5dvPN3WsAuq7pLCXqJEGJ0KHAXA5WpzeWd%2FuJkSL%2FWDZ5Amdfz3ClNgs98YXRfZRAdFkcWTDdw%2B7CWDf%2F7UxVNJ3J0UdW%2FHK6yMqyTohHNNtsWVEfEy0SkkNiRI6YmHbEurRkFcb2woVTGwShY5Rj3zXJJCCm6pBE8BIibzwfi3xECSrHbjxpK9V93AA9qMBlsEE2l1nWxcfDNbv5a1hezG7riabSAWTaxpnJB%2F2SoWtP%2FZlhdmCE90t0%2F8Wj2cr3CiPkyeSUV1PsZfNU1QLi9UVtXt4UOI%2BEfS5DffPSX9KNLneAM93OYa0s65sEzCvG8KHWA2zGHHQWyJXXKkAJRMnDuBn6mkliaS4%2B%2BPN2nzU99imYHzmibTAzofyM1Q2Qzk1ay0ZCbOl6cGMKWN%2FZ47XnSFOyGEswAzfCbN0l6MW9Y4My%2F3qZozzTslrFcmHtGTWKARkjzGcQL5zgjJYseSSCzOflW2IMoSPJJFpHVBzDl18rHBjqkAemafd4vNamiPhbDUVPEbIRGfASuTL98RYWqkUaVtCWJ%2F9UdVBt%2FBHkGZbvX6NdNt1WqgoawE8LBT45dw9r5WuUFI%2F1fwXJsb0Y%2FDOafhe1sIDkvHf%2BN6ZI7vrfeqKTzuFeac3Qn79WS51lpgBNYS4miVklGko8DrnF2mdDQzjJRJSH9vvIuJIXIp1fiqCbP%2BZozXvMM3WNbuu%2BPcUTZTD9QrVAZ&X-Amz-Signature=f7ad6c5450383f845bd858011a4fbb3b814ff7665b081c90d95d7b32492c585c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

