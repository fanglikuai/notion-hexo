---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZE6PVUJR%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T110040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAcyYsRsIfYpby%2B6op4UMemLFXlSu3L3cFZczxwCO6bZAiBcd7zAyixGoQE5Xb2s4TQZn4c9cR7jwam26yDAls2yDiqIBAiM%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMSpXDirPlyyjOlZGEKtwDSomERcEg5075cMKBmUWD6JapVZWaOieXk7OYQW8wpvs65H7xvToz6vvWrLwtcSrcE%2Bme1cAcEhjNfPhkVsgDaSY4hYKJYiHLs06dQzqAvg38y5uUrOXHI%2FjJTjgwn1W%2FlGvTaWnAzOPtzKKKHcAfmReldXvw1F5omB0yTo97fDcNwvze2TeIW%2BPF1xXMf4ArCL0zBxnk9E2744MgeRHIy2%2BPGZhFCO2aIcRZ1ZzuvcEeHl3VtlhyyLchGgNkqPs5qcuvnWwvoZf8WuXQ7QrDUPoStPrfzOk1x5UqPuYryU6xsCVFqhUiMj8Q1l4AAoF%2BdhohKildnhmT4SZZNSAbuRXlTlHjcJUT8sObYBU84FhtxwUCdVFsSu%2FzpKtkvqUppTTT4zqWy5Smxhlr7wAcxQaombnT%2Bh%2BhHaCQ5H49UEx%2FNqO4Wwz6n5HBAoQ6Oy8nPN%2BOMZMbRrfqzxqZ6uYQdg4ZXOMGCT0ZxzLgmXFobHRV7loXVj0UVOi%2FtihaA5Ka5cLzbHC3aSO9ZczZQhoBK1FwFsyeSEuILL%2BGFceWKq4lEPhVFEMGGHgL84QqLhjpH5NsCtwGP6p5pS12m7cH3X%2BI4LqPnMuMg2KB3wQJgzMx5vGyjdOs%2BiCg8kMwgs6syAY6pgEKRgT%2BVA5Fz%2BhmVnxZQXXgi9g%2BByCro%2BQfak%2F%2Fx%2Bsu5MDTeqkEcWJRQxcj3utX4a8CfqwHtmaYP2HgnDYcDmQ2jGL31mILrUsC0A1JHKnIaWF4XA%2BOglc0xaHZMm43W3C8gosvjpIQKia6fKlKtMSZt5SemdabMSEpoOZkiCel%2BKHlusgulgG1jRq%2F6SrPCPJYQeRi6kVXqg451HWxb2j8pfW3Q5S8&X-Amz-Signature=0489fd93e53045356eef6a1b15f59936cc27f188ddfb2353e9c43ad0011cf566&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

