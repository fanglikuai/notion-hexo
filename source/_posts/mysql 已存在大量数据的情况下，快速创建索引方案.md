---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UVO2XU6I%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T120044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFQaCXVzLXdlc3QtMiJGMEQCIDEuLqvVersfKSjWj1vOR%2BZgaxiq2YglH3jc5AWWb2EkAiAbjPEdpPQMWntw6HkZwlflP9UXCBatPgwK1V%2FDIuEf8Cr%2FAwgcEAAaDDYzNzQyMzE4MzgwNSIM6r8xUOEW1Cn28ZLrKtwD9gBp6P41DXUZl0xb%2FMytkP5vCtB%2B0RtIVWs0GhtgGSqiRrdjphQhe%2FkIxgJDM5M7POMOJgOMN61qps5xN9Nr5dLqkQ50zvroN44U5eZk9P8fAOP8GsC2sc78onr99c%2B7rPat9ww2uU4NBYu3FFDgFoGJoq2yprukckvqsyD7YHmxvJdaK5HNmVDDLDZ9xLV4%2BVxjf5Avk4NV9q647RhYtuNjvd2XQfIftkSAMUJsCWjKAIkCGyTrbXfpRLuRWqc4A9J1YooJ%2BSVD24Qex4Q1OuCPITkmig9gRVfaYLL68ZHyo9RCLVIggDRcce6VjOd8GBXbPXGcy0wA7%2FEBOLEDWjcshYArr79ImaBfydDnEMZlL0X7Ncr5gRgRZWHa740iFoYPpTPYfXuA1cXbQ2VXE31zW2bcip2zNFsmU5CUyqll%2FCt1MdX1NM6sHu4qRhTnG755FDODQP0BGKBGAvI1d644xwB6BiwIox9gpHvd7NKAxPzfa8ukBDoZtD3dG3fhF87qwe9ih8RDRqtMp0CCVmHACx4jrzoKAHQljClUmDvUb1qtmh2yFoXgaQLi8GBvEVY4RXOJyzbEeb2zcjTHpTcQMPSxA92Vp%2BDhRaYQ9MwKNZrUqPreAjN%2BmbMw67bMyAY6pgFQInW3U%2FdyIPvzwnaijOSaGzofOALIIo5fB9CpNOCVjL05sns0nJgX%2FpWVMWDvlsZB%2FQZ%2Bto1bxCnhUPv5T4lqSTm7ebc2XBTaEVP22AvOHpahvjm93rLuHP0Bvi7vQ6zZ8kZRPN1ixXiKjyzHVLnfPNSM9Vtxh89Wx2ueMSwLnor6E%2BLrq1M6ZfJSD3C%2BnkGM5k9f7QUVRzmH57WN0Rgg0IiWqyyD&X-Amz-Signature=f3c371a09fa5de93ac329633315e7f2587642eb3a1cab1e8c1185404e629a804&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

