---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665SW5QCB5%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T010047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJHMEUCIQDoz9aWvnLCP2e3eqbz6agMVyQz0%2FA3%2Bk6EF0sZMv3hXwIgSnUcczFfEG%2F9xySepsQcHyBzZ9F2cx%2F3nXcBFlEp7m8qiAQIsf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPcDC0v84M48dQBmKircAxijXnJKBrPn6KUpl4r1eZ%2FWzcK%2FPJh2KvzhwJKOqb6gFARuHLDNrRxf7MFfuCynBflD1H7o1sGRsGQiTNVB2T3e%2FTr1VahZNTuHaTIqbey7vOWiWB5waZ7VM6%2F4Yd9Kur%2FY4FuSAu5W%2FaXTYWDfWxds%2F%2BHnWDHDcXw%2FjknKm0peXhzpyZUpVYJHEJ9s3BMrA13ouZ09aRhouXT5p7niQXZNR5Pfiv4qtr07ir2OBeNRqUAKNrQyp3MZNc3cqzf2Sl9i3fVR51ZzD8HfZOMeauXXlo4vX8rWC1rihrpjGG7dZD%2ButawgDygOvGO0zmnFC9lZZgi2AmnhFdqnaXp4o6iuiFZQcxDadiQtPs33DNED4f6w6cPNxix2esq00cS5PBEiLYgtMAgk7xVOo15k8Z9u%2FEhNPFOltwK1DMoXEmIDFeMV0cuvfaQadoqCPgiyhbfcG03MSrDgoVUCGgnoKafwPWooWtP5uMoU3mijRYhInTilA9J7tKx2fPzpVqgnzpd6NaoINaTrYAYwhCwqfF4AO%2B1DbxhvwKYb7Q2ncFbRQE36C1nGgNZquxnvMhHpyOl2IGc4F%2BuozQGEs2mjDq4%2Fbipfk0fuBdm56vMasIA%2BblXqeVvSD1tGFzfMMOKYrcYGOqUB%2FUA8mTBtd6muXL7UooEJE4r12BFKTeUhFJzMY%2Fae%2BaoeacRLQ3ER7ZEZDk%2FF47ZcaGBeWwxEIVhsVH9qx2cSh4E0eRSaJnw1g08t6VZE9KDRQte2de9OR5jij266Eq4FugxYt%2FqIDOKWzTwCfhG4RCaNe7WIweIbQuIevP1iRHuusUJcuruLAxNwQsjC%2BPC6JVXOs8dgGO%2BiTt5kKDjp3F%2BlTKHo&X-Amz-Signature=d197d3d42c19f6bc07573698a78b37ee4a04e668e73a1cf31edfc408b684508b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

