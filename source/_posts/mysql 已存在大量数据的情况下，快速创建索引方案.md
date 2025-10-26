---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZYKXZBA6%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T140113Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICX%2FQE40l83Hb4DktKXZ88oZEZCYRamhZgOEfSdyF1EXAiBVUwVWGpF%2FDqZqScOziSJnjVAeq%2BXTN%2BuYXuUlQ%2FFS0CqIBAiN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMQePeZLaOGky7Bw6FKtwDEaCQnfFzX%2BY%2FmHdDBVkDHZeG8FCvh5s8ph4B7lZXVGKVr1LryTalE9mlMsoS5mkEzHqvgVPQcbOeBeKvHMBRyQ%2FX4ldLqzF25l8pKt%2Btf%2FdI2qWsVzHE6R%2FlJGxtRwizhDp7hPGbVKIT%2FkhImFyaix7Mft5cdT62aH%2FZ74gDgVj2DAijJWDdMqe4vZIFf%2FiahKgmaZUH6cILfTtOv7PYEZov7%2B4rDOABze4VpbqjIaOzNyQG9EvIS0HYcBPFHP%2F%2FQQdJD2xA3%2BKNxysbTcb5SAnTxbxc5ZgdGAeOEOjKamSbwJMIIb7AWxEcFDftngKeybdKK%2BvO2o2698RGnm4joXbvKVYOtHTpF%2FYsidqdM%2FP5%2F2iL8Rl0nZ%2BZt3P3J%2FwBNBx%2BmOy29x6OBVaqRUHGwiimGIIP%2BWIIsJqg5HshRXU3Ghl4Q9hJ2J5g5x55BKDZjY3HG%2F8wGctXHeZfdQuVhSYRh1sAh0slUJrEzt4A8dcBOjEgUledOzR9dTAyYMwYjPMWYGaMuCbrMnmZtSWEoZi%2FMywRgSvDQCw%2FVvX8etLOOWJTR4UUH93wbWLLzssbyYWQUUBpOv9JlsnLnnijy2s8iTJ3C1FCeSXDLEyBJTsEHUjtQOw4M9OXHNgww5v4xwY6pgHYp3wKWxs8ZWcdwMGqtwoixfGVQy6Rt5emVukwvgt93gBJ3HTI6KVf60z89pbSURfXygu%2BK1Oo%2FjN6D%2Fhjv0OfsgpDITACTU1AUMFznyxFXrSejIIbwj46yE4y3rPELu8pUSk9VQG3dE2XtFOBCO89SNd5EEwB%2BRhUuC1RDcevV6QRoHv4NLItiuP7IMPYp80sXzlE40ZiWexud0xC5nNnm0ePQY4l&X-Amz-Signature=019a098b709e59d0a026b6871ad07ad8653c532776ba284965657d5b6b641a4e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

