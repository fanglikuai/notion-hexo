---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667LU33QRB%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T170049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIF%2BlwJ%2BZyHb7Z4n%2BB1hjMPqJQIEm75Vl0ouXGC7MUbc5AiAOWyqXmw0RkBSpkrSKl0OvL1oT3XPiLhiynir3DJXLKCr%2FAwhiEAAaDDYzNzQyMzE4MzgwNSIMjWZF7jMFuImdb8oMKtwDd24qK94ZuzVM6ilZ3l5j%2FhANTK3y9m1lF9dZFgS3kqx4ZBIG9%2FM4VFmdMG%2BrTRNf%2F1iZTZT74KF8gHht4dZgEfTKBWiQaMCI2dqILpRN5qq9WXgKLgfiZ453hkCypiSBsCyiAbaNDrZjmvaxm7FMyo0WN%2Bu9gWtwgFdMG%2BmqSCG%2F1sPYtM5HmvE2JZJDforLps9A%2BUsZ2I8TKSRwgAYUK0nTBJLTUDzPBnNhe7ESWPxfrsCNif1%2FhY4C6i4mWnIbjQBpeOqt1aquNDJFCsJT0grV%2B6uzLzjxKa4Pm4owlN5nkHJkPMFj%2FkAUhrkSzdnl77SjdtTGt4DcFHKCrKhcq6U9r5VRImzvzRwD7L2nZppqzj%2F9l4fUBnX87GSC2UrIxn8s435ae%2F%2BA5zrDnjtOn2mxNvnD02KMHK9cowfKwV9Bs9VuTwyrIeL6eGoaZijUK41%2F4J%2FG9iRzThK5lrwwzVIzh4RWMlEx1CYoCQtIlLx7I6PgN4HME%2BdRLM2xUFj9uAKJSFRpK5nlFtvkptpic%2FSulQzktJLTGco0I1PSrRJMa4xvUIgdi66FJYhkhKqf%2BzYQQb0gukOKPf2cORUyQaSg819PKukgfrB0QI8Usbo29azuq65DO89tnEUwgr3QxgY6pgHITb1Ojq%2FiIKW%2FrSOUAh9aV4KqmDu2Tjiu7mJbRtKmK%2BpzPAkpOjE7Z0OXhQv36Yc%2BiIBD8L23a7FiI3NBZLfORV98IRWizSkQVrdStUtzQyDTS1i84m8iWRj1MErTr4tT2vK8t5O5GfUJkqZ7k5K9oxEVUOHZgOVq%2FRdyV%2Fc7HvCZTlzG4YTLuyWO0Knn7wXKUnXiBskulYTfDGd6TnM%2FUW00DapD&X-Amz-Signature=0474f11df969ddab713d2b10fae8f0e3e6446e4de7c72d9c7e554bdc7bf0a1bc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

