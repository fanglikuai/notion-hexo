---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VMCMLX6C%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T020048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEkaCXVzLXdlc3QtMiJIMEYCIQDQS3anK24sX2RS%2FmDvfjYmxNkcupoI2tUozhR519iLAQIhAMHmE111hc62R1gxe8YsnxyGJMDFnRQd9Esd8hbjBsVqKogECOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyRE9R82%2BMHFvsJKaYq3ANt5D7VTloueVM%2FXA72aA0nnVjBCDtXBEvjrkjw881GKFAcYhiAeGlOZLD6Wboo3QNqOCqiIXgW3iFdAGB88Etc2a3MjWV%2FYheMhQMhGGVqA%2B7a1Va1sWNtqEDlSe8IHe7IRaqsnaWYt0ZZ%2FZWOXlPmvMimkdK7WdI2YV2LtZfgErHBCGdCkc8JwaIcdxcH2jkwXbR9%2BoP7WKgRUzEZM0Mpwu2lXu%2BIXGkdZ1RHy1A%2FsuSLyYEVMd73Tj67hOP%2BoTh%2BhmVGqr2jHbVjK%2BBae%2BWogcLUWF1JK8LJAKJVuYg4YuhOT53eGYUbwFFnfdsYmp%2B4p%2BLnGqR9knRFusggXEcnOzza0O3D7%2BMept9%2FEyWjg0XJJDGJWwewxg2%2BoEHiJuf5iWceLq%2BAYGAbW099D%2BLME9Ok2PqgpjBqAyHiq4%2BiTwpgJan1gPbJN5HrXvZtLm1mhMq3UWa3FUnNYlXEHpYWSY2iXDJvRRD8shI25Aulz%2FXa0zRa%2FNasAWp1gAaOuso18VHm3yAU8l52QHt0iGK6%2FT18latVpfrf%2FCsZUS%2FIVezLyIIG65mEDh0NH8Wm%2B3NqNQFkdbv7TZdq1nwNAuu0%2FwrUnSGscAeQVxLPpdC1kFiM%2Bon6DUH5MpUAoDCPsqHHBjqkAUWkl4J0KNc2BzXzSIyqDhgHdVOOKdvwNpGuWvz4f7xZaoYO0llkftwiS4kRSzMRZ3LngBNaRWm4knzi8BeOIgvNwOLNwU12R4ue5Wib7FMd%2FjPsz20SKQfJ32hae%2FIIGVZmMxOzrE0btYw%2B%2Frj4aJzB5GpKVmLZSo%2F4h24LXyGZzlgpzLqeoLQD%2FfHoc68x2tFXVHlx0M7fbcYWOKOhGc7Vo8Qi&X-Amz-Signature=89f7eb5efa9f281e3edc03a95143accef275a57584c4334cc51d79a43542e5e1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

