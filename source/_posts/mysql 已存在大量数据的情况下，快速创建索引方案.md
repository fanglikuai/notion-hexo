---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WYIMW5CW%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T060122Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCQSJBwMBmj%2FRTTAb2cHShLD74t1byXa1UgmgjUhCFI2QIhAKDf9CHloiDNGSfqT5o%2FQYAPecuTdVoMlKY7mJ0H0QcMKv8DCD4QABoMNjM3NDIzMTgzODA1Igy2e3qgbSHLcIrJV3Qq3AMjP8oWSz%2FX5VZLTixm1YZXU0cWSK4%2F8p7CWSUoYMBZZpTUVWbhKcyBiGPeHr7HTtaNcVHLoBVjyxQrIxCB%2FmGFTcrnMhk8i4tk8EEYf75zoSMqVQYrpeY0juPZK%2Fi%2BH%2BWCt0AGUYfRA06IieUzixIDFcw7wkD0ItahQzunaxQpdUB9f0UKeX9niQvVVLI1NXI294Uxc74KsGLFwdY4bqMuqiWLlvx67oqrwWnvAOcP98UxEAGmFYaA%2F%2Bp32VLqGtBxbbb8oJFxj8Cd5Hkd9MuQq7uJat5EQDT03T2LOKaOVjrgz57LEOugF3j%2FnnYTF79V8zNBaBAP5AVXlUF5RsIm1eWowTbaGzazCDV77h1pYsJLV%2Bug9z7t%2BMYg08qMy0IiYhTTk0Q7dVX1fy9dvQVWSyJPECXFVvaOcEwVM7UdyYw3VmOeNljMCDk%2BmB0dX%2BY5CjEa4Mz1v35xDvJbuDV0paJyx9KWlDJ4hM7BsSVMtIJhIYHayvVY%2B1V0A5zYoZvcKQJMXlz55Oe6hDs%2F24jDD5fgRcv%2FHp1YSvgLcn0IGJ2VW3WlZ%2BreHUTWlzdohzs%2BCgHAPfSVE53BCLUkChLehOYDR8sHWuIFsV%2FWKvfos60IdReymdgEjR7yJDC%2BrP3GBjqkAWoRvBw8PEXfcKYVQhnEkP1TuQhf5da4xytggw69hCJhMDC4lEUkgywy%2Bg3Septdj1BiA8GGQy2AzA%2FwFTzcUEZG%2BmiF96K%2B%2Fb6pWH7jubtElLLB3aIjh6DTLwVzv%2F98d1nikKvdDzrBLNI1UrMdI%2BbhtZFycc3SyoNIZqqyTWcJkYioBlTbV6Q2UJdMJICspfx1i9%2FYlz4nq0zg5JYpurxe0uO1&X-Amz-Signature=e425a51770786fe94f45fbd130af7843ef7b2c262471f7d369031eb59d88bb01&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

