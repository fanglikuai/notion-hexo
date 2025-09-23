---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664NQX5CTU%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T070039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD4QBcFQNj6bMqcG8paREi4kWmt%2FgjSgNa6EQjNx8cK2AIhAKHkZJ6uQfo0rqHPx5dNAfjsuhDb%2BCc2EQfHKI8uVuKvKv8DCD8QABoMNjM3NDIzMTgzODA1IgzQhFyF8D7Pz9Bnl8Eq3ANwS7heo6%2FHtNpVnYtUjZLrthCJ60TVoQ3ZjQBRCnm8O%2FDs3PJxEhx4vTNiqyDlR3Pv9T40EP5gWg%2F9uU%2BU8Q5JZ0%2BNIm2RCbNQ4Plkg0OnWuxg4cbe5gDXBSEHKgjinBmXOjB%2FqWe0%2BBSOCw4i%2Bqem5Fu%2BWcDpUevO2f4NEYGzWL%2BVYfDtAc%2FOIY2DaNhGg4Upb5XXtEqlDfmYf35i57kmHdMuNAfU%2B5PtYZKdeP%2BMuilzVh6RzSNbigsiM69jYvooxldN4Z03xfcJZiuFXiRx8d9sA7NVQGaoR8XtLG2omaHd0yiOJZ9oVfqNMBsNUlEIipRpj3025Oi27eHzva6i01rYhLIxncpdgTJivYIcBT%2F8PFKKNjmPuVdPIePxPuoAM8ztzyDbXMD5F5Mp%2BRLqDlgPJFEKjtnsk3aVO8e7RthGUUiXbGNd1pLdvTl4QyiV9DinF7%2FmPIMndmkfdtonfPLPsPXGC0p%2FxqaG7lag4lWkqobm2ssvyrndN3pSyhW15ACKo87NpUumUsC%2BBb%2FaYHoGae2GPet2oIAg6Opo88k5YiqLjCoysPXAE2wZI1rbUawhLoSPQ9tH8b4PCBNGRQS%2FkzgdrivDbcEbkqpNMeL%2F0Q6bSilDJZGvDzCu7MjGBjqkAb8hpCoCOnzsucnKv1yt9YjttmgEsgiDFeFGatSp2Qq7G0uQXa9TNZdrUZ99%2BXz8mJEjKTnFJ4k1YysEStI41JuFxBkdbsmz17x7JCroBq4nNRu04nHzP6bHOTr34ISUv5bn1J%2FJDZgE4jzsBL6I2j6%2BkEl3GKV9WaYKChdY6t7yE1fxwBqgSLWEcm7x3bruY1yIjIjzIEnf%2B5o0MMHWbszG3vxv&X-Amz-Signature=aadd735e261327abb914f428ef657d633b4e6575b6f1db061487e1b48ea4f0e7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

