---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QW7T4ML3%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T190043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJIMEYCIQDKQWj3bs8XF97fvoB6Ld%2FrucfbqMRto84reP9DvljEBAIhAIWF5D706ti5wC9lmdmNdVIPnEe7NKh%2FTBSml7HI976yKogECPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxDRi0wkDt7KBNPdGMq3ANtkX4Os2q%2BSh9V%2FoNovJO9q1ZaroqT%2FHK%2FXkCLh087%2FhT%2FrO0bcjq3aYvC2upI0Ynp5e%2FPyiTgJdEaRVzOOWRbg%2BzHhkNxZfaeHnSxoV950dlUVaEQ04n9UgVDB5lBvRv79p7d6A6esPH4c0ukxoYISU7eKUjo6%2BdfZLAWBBuIkaxBWx6Ls89bGzWDJo8JZJEYej2%2FpEzwFMw9EQ%2FVtLFtMmFqas8PRaYfpmoc6745rtAYCsybUaz%2BB61X4MM6T146ErxfXZ7uiRMjtFucTrbRUwksezlFFSEgCcjPtFg0fhbburcb2FSuIscDt2eoY%2F7EdDlD367FUlspsBqXoZA7CZAmwqfXOsV9pgxTVZdGGLIZ%2Fc1V9HadvovM0jZfy7wlsf6cXvhiWwNhza5NT%2Bpwc2zahpQ8HBsD4%2Bjf60leTQ6MkPSLrmSnkwCB1KI5%2FW73VVSwvPeVXTRgdPOXQrqmB%2FuRni%2BMaFhW0qQaMg2boNhoh6wk8gZDX1xi7w9azCOHk4iuIgbdTWo4NnGCnMC6uthcXbVzByqSbXdM6rGLvjp3Nn6aMncbbBH7hRoNpxJZb40QLX2lWVYfYI%2FoyGNOGZaeaAAEFVbrxLiByT%2FMaOzKmvSOxmu1%2FzA5UjDzgcPIBjqkAaZuyjW%2BlXJAmvmqdHYpSOq6ENfjyZaGqBaozaYj6vBNqCktjyoYYIqOj4VLCzCv8RVlMcCjhx%2FpQoNQ96I5o9j67sw%2FUZfo3fySuxez6BglcrEdV%2Fa%2BtQFX9r0F00YSyjbjiuny%2BRSsu53fhNT%2FrSlrL%2BIPwtg9R5VbDP0aALfsc63yuCIx3H%2FsxG9hrLCqFaO1LQepbO4K2ckEp2XLbHepbhvm&X-Amz-Signature=4f471b9b96d20fda1880c59fbbc4436bb6ca10c3cdbc18adb9464c0c79858655&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

