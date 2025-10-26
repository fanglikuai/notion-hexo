---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZYKXZBA6%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T140113Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICX%2FQE40l83Hb4DktKXZ88oZEZCYRamhZgOEfSdyF1EXAiBVUwVWGpF%2FDqZqScOziSJnjVAeq%2BXTN%2BuYXuUlQ%2FFS0CqIBAiN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMQePeZLaOGky7Bw6FKtwDEaCQnfFzX%2BY%2FmHdDBVkDHZeG8FCvh5s8ph4B7lZXVGKVr1LryTalE9mlMsoS5mkEzHqvgVPQcbOeBeKvHMBRyQ%2FX4ldLqzF25l8pKt%2Btf%2FdI2qWsVzHE6R%2FlJGxtRwizhDp7hPGbVKIT%2FkhImFyaix7Mft5cdT62aH%2FZ74gDgVj2DAijJWDdMqe4vZIFf%2FiahKgmaZUH6cILfTtOv7PYEZov7%2B4rDOABze4VpbqjIaOzNyQG9EvIS0HYcBPFHP%2F%2FQQdJD2xA3%2BKNxysbTcb5SAnTxbxc5ZgdGAeOEOjKamSbwJMIIb7AWxEcFDftngKeybdKK%2BvO2o2698RGnm4joXbvKVYOtHTpF%2FYsidqdM%2FP5%2F2iL8Rl0nZ%2BZt3P3J%2FwBNBx%2BmOy29x6OBVaqRUHGwiimGIIP%2BWIIsJqg5HshRXU3Ghl4Q9hJ2J5g5x55BKDZjY3HG%2F8wGctXHeZfdQuVhSYRh1sAh0slUJrEzt4A8dcBOjEgUledOzR9dTAyYMwYjPMWYGaMuCbrMnmZtSWEoZi%2FMywRgSvDQCw%2FVvX8etLOOWJTR4UUH93wbWLLzssbyYWQUUBpOv9JlsnLnnijy2s8iTJ3C1FCeSXDLEyBJTsEHUjtQOw4M9OXHNgww5v4xwY6pgHYp3wKWxs8ZWcdwMGqtwoixfGVQy6Rt5emVukwvgt93gBJ3HTI6KVf60z89pbSURfXygu%2BK1Oo%2FjN6D%2Fhjv0OfsgpDITACTU1AUMFznyxFXrSejIIbwj46yE4y3rPELu8pUSk9VQG3dE2XtFOBCO89SNd5EEwB%2BRhUuC1RDcevV6QRoHv4NLItiuP7IMPYp80sXzlE40ZiWexud0xC5nNnm0ePQY4l&X-Amz-Signature=c2fc6be93e50e7fde268e1754bd137539b4faeb76cb2bfea287984216bf8c23c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

