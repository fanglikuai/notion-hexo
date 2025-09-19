---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WHIS3KNH%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T010038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJHMEUCIDiMydSiO1FjKUUfGZdNoJrWxTFMADfjx86pHO4QUjd5AiEAvGuoGKFTJeH76MJfsXIPNrPrJ90gRVQKOCeIWiCSNTkqiAQIyP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDN4MXziLHffyFmjkRCrcAykZHzspC03DTISpOHiIYsFb1hgA%2FA7hZe%2FCtr0I6RxmxRs0ZIs9QKi%2Ff63ghSnyUmcc8877gL0ETUmN8p82xiRGkafzSnn2UYucBEkZKu2rSFy9U4tMCOLzzOB7HBQjfGLLpe3EcbnsJ4IUt%2FeEZ4Iuy7qr3QpVRRmxY%2B2t0V8GIIEozNwrbyVuJgRlS81eVCFvAvTBkf1HWvenvHt0sj3%2Byl8uOqLNAGu%2FqnUu8z0afBKUz7jWudRyRL7gcy6lfsu3y457K5aFuxRQEDUHpILqOPY3A%2FDwMZjOHQpUArUusm9Vlk9%2Bu5SoRnWlD9BhRwLk%2FilbjIW1yUeXlnbbh%2Bz6iea5MzMBHqt0flP5xduxGLFIXwzvk2l94Ng6fPa%2F4BSn6hp%2FrlSY%2BaW1vCk8SQQqy69%2BWf%2FumCe09YQvu2cskS%2FT3y%2FCoZ6Ez3TKduKOOFQwJ5FZtz0r3qDz6Idb2O3fDyk7HKUNepQ1u2ChQjYt2D0%2B0382ZT%2F1Elx4wifT6NjFqo0GyZVtRwwntXJfbsqAfsGyTqGBC6LfTddx2CuM3yl0RihXomnPefAY1izpLwvkuOGBIxdzv8htdMm8oactVP0PkDnVJ%2BZT6l4KPvxAWx%2BRbf9v1PIyMmnoMLidssYGOqUBRSHi9GWKvpFuwO4mhbhCAKlh2yRndyqmXLd2VP1XWWvmbM2I2wMjWBjliWxl5Jptex8ha0bKbmd2BVX%2FFFL1syXlyd1tPI7Z4Wd86zOWzW7li1yHpuDP0wg%2FOmvbCRgFHIAsdO%2BHwk%2BXKxJn7bgtz4PuusAQyVDABSi9m%2FoL2HDht7xJfQqEGaebGzjm9zXi0SS04RBUYw1RjljkFP%2FUuElmZxQn&X-Amz-Signature=2cde922a06e41a82766f29f484031b18a67a798a72aa94803da727ba3d64b304&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

