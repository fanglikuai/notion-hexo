---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RQX46V5D%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T150045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC0K%2BLMbkLqnEdUUMIhHGtFfW4iRVnO07dYrFg7vWFGOwIhAM5bQ0iFmeJcLeHp%2FDbluR3IqazDc6qd6xx2z6okwwgHKogECKj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyXPYwuhKmoeHTc0Ykq3AMGzo4KVUACrDyTQk9n5DmDEQqDlYAGiassKo6tmIPuiQsI7FCbgLeZePDlyaYz%2BllFnjFsxt3R4lGtPlGIT7LacUxoFCnuhEaipmRp8G%2BJbBoW4Fn0rbtrK0cDWT7zDAYqRWJrjWTfJJPe7UObmPe3UJpvG5dNlPQycA2%2BN1%2Flv%2F65DirxggJloOpCZTu3FV8wTKzFRXs%2FY2OvTfjDacxvdKkVTV5vTIRUBTjdqe2ISnuU7EyT1ixprHoSe6AO7JLQ4hLc3WOlYK7TrEaALeP9tdfYZAtBfV6ru%2B5KqQK4MhzG6Yd9COEJ0YHPoUui85tqiZvz9bfNBpaVFOPJRuBy6OpTrVsp2Gt%2FpayN%2BDiQK6sQx6z%2FmNWphVjUuVwUlQL3QBTTgp3UnVXTxAa8Q%2BJhD87x6hA1P3CX17xhxmtppJfooF3%2Ba1xj8ZjjtGeI38t3liVw0%2B%2FY2eFc1tGSA7XGqyhLufIawjYZqvdJvaN5a9e0zz8Mrin6HcLkvyZ9TsyGDtvTnRE2IWHTpGk3EgF74eStymxN6aDxdgvsthz3bIvozhu%2BFkLhtMRkPBT6wKQ8jLBVatBxfWajCGsf8pG0VwIXuqTiVVuYVLjJrGZ95V777hPzlm6xKwbw3jDi5LLIBjqkAb77p3%2BpqiQ6CLac7ZabAV5vVtplOUUw6XjOQmszdyqnkIHr5C3o8SbIea1EXtQK0M%2BjIH9NmWQxo4iJ3vMfNTmkUTgu24rsYgz5j37aSw575HFWQDXfoFhkP%2FL5maKXZvicEDCa1Kk9c2ZNUqB7Ka2tGVTW5xWHctN%2FsvBzTqrR7wEaDO6WHa2Z9O3Pgs3FrewHIHlGa67z4JwGDp2EJh8A1hBR&X-Amz-Signature=6fc341d88af1470ceff9d6e44fe7e45618e98076b9dcb9270366752e6887674a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

