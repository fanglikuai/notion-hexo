---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664XSAB4U2%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T030037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDI9RXJvdszKaeROdc%2FdKaodqh7WdTVEcVggL4MLhIajAIhAOjyWRoVwbQO9I5W5XbYkfV1CZaLZcWJUvbTj2kHFAdwKv8DCFsQABoMNjM3NDIzMTgzODA1IgwK1gUi1d5MOyhPD3cq3AM%2B76NjHRt1I%2BUmcuImEsQ7Zak2GknV7z%2BcV1yfe7fVOJghi9xaoS09%2BfKqSHgrcfrwRc5JSRYH8CaN8a88h7Mr0%2BeOrQV%2FnoXmNLwv%2FNu7v2LjjtdfusqMv02mhANnUcdz7l6ztHQohg%2FZin%2F5IjbOA2Rv%2FYF7VN1szbtEoiBHQmMRSJ8%2BoQI%2FOCh6UduaC%2F6b0vC5iNLhVQ3iRb8067p1jOb0A%2FnfC5fGjnzGIvto%2BW6IlU0OW2faRZaAjBv9YMqNedB%2BUUdUKz97dDySovGOW1BRpGQ2XDRFDygCTMExJEpf2myCbMiO8dZrhyI5NiGkTEJ1rDfaBY9jicfDk9uDHOXDl1G36L4NxWAuhbMGvvwmJB97fPQedf4bap9AcFnu38CfNl6d7qKPycOhlQNcF95CCoqvXxzDbkFh3Hg7xuqqEVs6otnBbkxRLwA3P5Aq9xgr6%2B%2FIijp03%2FP0M4ZebGLaV2wCXA%2FYRxgNxt6I4cuS41Rtxzd0yMRokmlLW1z7CADDew9FUQlUKiaDGRPFjk1OBXZRjpWlguQkM9V9UoydbIFlZsJyPbt76dC%2BD8qeq2xXRHzN6TPmkXhiNKzytsQUvxc84XyohAYFxa2%2FCxT89VI5r9DkS%2FNClzDjk9rIBjqkAcdnXmpY2%2FR5r0zDJtICk1x0s5lHR0AsTHaguFQSjzS32IQ43OclMIbaogezN4gL4%2FhA6pyp4jrsiPTn25ZIX5lUaY4wqme6EVSLi64hKeMjVsSTK3NwTtnrfl5y94w6%2BTG403bS0oaSpUZ011f4yg8nKeHaDGQFY7x%2Fuc%2BaP%2F%2BSZEhPmo5N5WQHrZGN45hri0AjmZ2GdAOUrOvx9S6pEdnBEdmh&X-Amz-Signature=ffad11917c7f1f7040c61adfc59231ccdae2d5176f1aba26517701cfa08efd7d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

