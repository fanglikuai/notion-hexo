---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664UFVQNHS%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T150055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC%2F659yUFvdJyjXsXj3wnO0WMkaf2rrRLS7j32bKUxGqgIhAJiEw3u4fCsnPTQStyYupIxrOyVxk%2BXrx6wsOXGl3X20Kv8DCEgQABoMNjM3NDIzMTgzODA1IgxLzSwJSIYvz39sjukq3AM3hG3WcnK8v9Ib%2FImsOiGVANPdhI%2B6ecF6AqaUS1vAYMAzjf%2Bnu7E%2F%2FnskrJd4gCDzUhmeY8vCHOHYVITVtAKja%2Fq%2FpYLsP2pccrYwj6xj%2F55YmSUpIaa42wAMMnOW3lsjYT399N64FZ9oaaxULbbEZ2rcH6rj7UjZGRbl4YgWlhczQgN8I5gYAYqckyWAdccts18GJybyRSCZcoYWctJo3i5S4VR%2BSMdpCDLVXSYbJP8oWVykZh8C%2Bu1NkQgjYgXzjxyxwHdbBq8Y%2Bm2uqB68dSWeFD5BCtFSDNrahYV6isvtb%2BGXxIue898gFDXj2ZsujfOeU0QEQVknpShNzTNK0hlOmi5ERgy0aPMRIQNG9is45nloJ%2BuOmmXmkOIgvkWcyuV1Y2SaSmm7p6qfKJP0op4mogIXO4aBQDZE99WAMmKA0sNnqRDpIYoRPTBZQuL5h3gOMd0Kur4XUH44TDv3QKRCpGtppbbEgUkdNhgubvL7Hun4CTF%2FAtryQGK3BQsDQB2DKQchjouvoBfuDUyjpySaEwkjXNMML2bK3%2FVo3Nt2QhoYuvjH%2Ficeq1ud7riLATMZf1KWzKKdWmQDMd5BOfsjx%2F8PCgEnFe5Tid%2F7uNeDzZDAgzSDSYb3QDDs%2BujHBjqkAUezjWVoG58R%2FHxhnnk6vXd4Mn2X8EGBTB3LWzl3KZky4tio%2FiDiLLTIKKAWB3krs590M4JRt31kQTsp6n4FwB7VT1ZwpfYPU3pGPTHDcNwtsGscHlkF%2FuGMsD3fkK48CRN3NHm00ZFJ3qiYQ%2F3NYEbZoX9b%2FIwXK6XqUF19cKyqMvgErC9ll78xnKMd%2BGJbc2%2F8sWLSLdF8OnU9%2BaQjwXbAdueo&X-Amz-Signature=eec3c7e057d63b89ca67065b0797484eac5797e0abd8caffe8dfef0478d9b947&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

