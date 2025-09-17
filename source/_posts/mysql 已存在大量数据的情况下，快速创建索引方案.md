---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XDVFXR2W%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T140046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC4aCXVzLXdlc3QtMiJGMEQCIH0n7%2BVm%2FAEkP7sb0MWttZ2rcg4%2B0bdIHRoz5owTC0y%2BAiBS%2Bzw9OxlMrcBaPI1JrvLjhnGkLTX%2FdP89uTUBdLxlOyqIBAin%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMfMGGF5uxG7VWNfAfKtwD6bL3Gr7tFnAbKDI1RZH3nJn8Im3wGytmuUaQ7oZl7PN37YY1zv5dMMbAIx2krr0CrShjfBzQrEfT%2Fh2dSkptSM63rrR9iGOIBr3hly8Vcw0KTCE1v%2BQpEaXwDX1GI0oZ3G7b1W%2F8fp1xlNGKNIusklhQ2kPJf2xCpum9C5AA9zjjD4lRtM7EwKVSSFYWBZdP%2FUI8HmbWlK8t1XtPghoYUzBa3%2FhqkmgbvfRE9v55PgOVH0jLUGvHv6hVLJw7%2FQgCK47LG0HPA1CbvsEqxSWQ3Pk%2F1xLGm9MfP%2FwWouO0M7zZiK2YRlkfbRZ0bYjTG%2FpuuJ5ehuuj0QJUd%2FsDEkTH2nNTi%2ByYfnamGJVxVW4PdOHN5G%2BkRHQQnIXlkt8ouijk7vugXdmwbFTs8RSBPYGC85046XU0bdjk24Y%2BrNjYETIDDec%2FLa5C3uhxHYQZoRh76YdoE4SQUciJnN1MENBBLx%2Fa%2Bxlbra4uMXVowpcZpn%2Fz40dFLN3hz0T31oi2g9r%2Be9xNWAY9h8mR9CefR59QKeAWdUD0ReeLvLUMwVVEcdNhJWL%2FkqGzU9NuHFkKlz5N0ZGpxb6topslMJradC426t2FzNVedXagGCc9QJ4QkapU1ssc%2BoT3r%2FRdZAgwj%2FiqxgY6pgE1w8XyPuaQIByhhWcxG%2B7unnMi6%2Fn%2FvmhKclOO%2BXLaWsbQ1qZJ%2FMdcdC%2FYw4zpMRVPoiia9BqOTEbeByJUQtHRYb4%2B%2BX4ZOmwnpnyGKX7A1uR562%2Fte8eBeZjs4SKcFi6lgpgyfoENrj6O%2BplVslwTuaW%2FLOLVsJLBVWDqk5ofDgTbMjnxOqU%2FDz1%2B3nNVutOVuvTh8nceipgSsmGbkfrEm4yz4Sw1&X-Amz-Signature=72335f3285a21ef68d442f2e38be530fa63523c3b50b85045620bc2fac6a20d6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

