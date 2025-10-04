---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q2CC2HJE%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T010040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAGCUk4mxHpp77kZBVoT0fgjwQdtRxffIz9fBhMKJQFeAiEAt4idbxng%2B1hl8yMUmvM64t8CZ2u%2FRWZENRouoyi2H6Uq%2FwMIURAAGgw2Mzc0MjMxODM4MDUiDH%2FANPpwinA8N2XPPyrcA2%2F4H0Hz59XCSjh97atvXNFsHE0ndEaQIDHVK2jqlEV%2BDmE4uPxD1fhCM7Ong0s%2FRIXrEgDpXuvJMcaWJi7F5qa7u%2BMKARUZf%2BoV8VqUU2CG8SErOH0Kiqzdvc4csRdsU7l%2Fec06KfmRLK4kNcEBvDjW1hPb%2FSBboJdDYbFYft26iQvx7%2BFMc60scMKiS3mLDwS1hcDBodgR7MKwahiLclwO%2FYdZG5W8earj6luCBdWoh1%2BqBKepSzQcgDtlUysKR0%2Blz4Ir7MqdZtbLKOTFnUTWw2lg8KU9bsjewbLKk2AQnqMqjrQ1bstwO6ZBi9EoJNg7o%2BoFj7AKAp0C7BHyIJlqxB9ELM339dRIW8xinbCWYNIMur53rk9%2BMHttOjKwKSXWtp1Vf0PoJmIw%2F879p1ftjPRgiTA5uBNABG80SlV%2FPS2vaNO2Lg0UbQRJB%2BPwZh3r6EeIBt7uh5G2Wz4Ou%2FAJCL1GfrEzxPcxppfyIa02qvrbo8G3dj%2BKNonCRSI94khahbdk2QX3H%2FzzW0nINEMplERSTj38HPkKrlIkjWMEvXH4hDgkuH9tHJ4eYG9xoPONe0fp%2BZ81o%2FDLZQBz2blFGzs0ifHpGLEVm5I47EO0SFydWMN4GdWDkOBAMOHPgccGOqUBBn%2FU1jJgYkdcWuzQethfm4ZrfI6jGHki4jdPZ6nD74Jw%2FHnfzLYCb%2FsfmfvO6lFF1IMJzTOscKEWVxl09gwxMiO0lGs9IGlSWW3AYae%2FCOYQa01sskc7ePEuZFc2lulMSO5CW2Ah3mysTDPZf18yoocfSsM9Yd2sDTK3iDuLRSs4EZvRHRWjCyf08GrKlw9jIwSB%2BvrY4t0l5tEor6GgVaQ7KcOC&X-Amz-Signature=f13e365d4202ae6a451829759d6ef32e97ca60e63bed988bcbd2daa18193c655&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

