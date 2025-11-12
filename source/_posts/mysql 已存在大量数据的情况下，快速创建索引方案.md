---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q4EO6D6B%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T040039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJGMEQCIFSDKruO%2BT1jy%2BGp16ceEUnBvhF4g9ilM7fnXplG6%2FzWAiApHus5ocSAhGa5B6kOGb3w5IJ3eekWlj%2FFKjsbX29L%2Fir%2FAwgsEAAaDDYzNzQyMzE4MzgwNSIMkCKFYhlvz%2BIiKQenKtwDx9hN6kecMedU2KoP4gFrXy%2Fa57j3w8QvkTYc6UJEO5%2Fp09PSLhQoNdMmahJ9cOv5N0TEWmmIl49Wd9%2B7MO1XTLW4N8CsYvW7msH71wpIK6I9jzm%2FJKYLnABVQ5kIDQVA8p%2BQqQFjLnZhZiuRD3Ke9EqhxHQE9SFgjuu4R3j2KsoInJUOBE3kBkq2ETqjAxo%2F%2FYhWQZeWBjA4Plk3gdszEGiKS2nFCmxCbj8MW5AELB2bA6bbphUloHJWsc%2FfIYYlYPt01OpnYdqj9eNKyWfZsKS%2F4a6eGLxlH8g9C22z8s93g8kaRvnzgqovLhT2bwkt1eQ%2FtYoTKBW9OE2PKx5t%2FfIgKqEAWNr2TSgsmLbA1j9Rrl1LRqpurhFQ4gjHMA82%2BWJVC6Wg6J8CU1QJaoFtGLXBzyNigT4MYU98qE%2Bb3bhywCMl3D0EOv0LQ74%2BlSP55qL4eQQTk4RbB7hr5HcE8cNonZcrjxnyrzkkrhsPcBCGLnGLAAl8YDK12bxoCoZ1qqHPKeHJG5S%2BrDOso1n5xc0EYaDynp3%2BNN5zdHi77yCKQgrPPCmiJrV6hYfrjvUKXucP%2Fie%2FAWjKSCnPG%2F7lRT%2BCBzK7O2tzcjYHVQhdsMh8cq6OYv8i6KwJIxUw%2FPnPyAY6pgGILR61R6D4cuKnvUTK1T2%2FCjvnd%2BSJ8xwl2rBmaosowaJBW9IKmO2Fd25kyYCRMiE%2BZ4y%2FoRr8aPxI%2FhChR2%2Bor%2FzGYX09fKENKrQlsXEoikXT9VlpW20qghP9t35QkpPHa3aRswLjObHeop65s2zb2doxUCgd9A96hblUCPSTFmzMzvcBUrK3L7G3UFmbThzKxWi7dCHFMhnf17mQmamZn74Hyhc8&X-Amz-Signature=3c7bddf495ea2ab5140daf51b543a3b76bfbcf303268b6292855f45931e264b4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

