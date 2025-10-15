---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZHS2MXON%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T030040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGRvD8ENrNxPCfGZ3jYanl2MIPtM%2Bds06x8QqwETIikBAiA4Dz3OtNs1hDiWiw7NEQ0G9LjV0abiOBqC9iBDajbp2yr%2FAwhrEAAaDDYzNzQyMzE4MzgwNSIMrvhQ3lk6IPadb9QZKtwDPQmelvNkULQgE074J%2BoKDh9t%2Bcek2nluesalQ3AGtQVE4GT0qCFsb6fuKHSkRpbBTv4x%2BrsSuIuyV1M7fDHlE%2FYU3WeCwh976YodBnLD6HqmOkoShFPrIB9SSZcDWgLXYZ8%2F2td67GsoGRKHv4ojISk7HQUyKjtq1pMbYUxsyeCHCi%2FRRc4%2BPneZiNfDXHOQKSwBh0jL%2FtkSu05NDrOa0ODbI8%2FDuxBFGP8%2FDJS1IybqbLbNbxaaSnTsxzYt%2FDjaEJfdVqiWx%2BioDXwU4MRlJWTyj6RuLPkxv3P3lRlQc4DGiKPSDZmiKkMTeK5VH5P6vB8uKdQui4pQpx8mvq3tRxtDwOY9IbwBaw%2B45zuJbpbuDlGEjM169TTPe8zIbymxGyVDjH%2Fa1cuE47pjsniDpwdz%2FTrJa9QW4rtDOKuEpiwPpM0yWRwzYZwGhEiRrVWggEPLcPg%2FLRvAfOnZdF4OW0iRXDm84uzP7z4rkI0EQg0UUZBsI4zw2RYyUK3MOiwogVCHkUD%2B3heLXglVErgZwhuQv59F%2BqB3sfOScJiub4U32MfYXBP9rLuDV8dFUgYwkZ4v7sRGxZntK9fYIYD2tta7gchKv9XxHS%2BOrIGO4zpf0fj8bzwJPKo%2FcG8wo4m8xwY6pgEvN1trGwrmqJtliQoimO%2FlaykBXQBRAiQUD3r7xD6rqE6vrNdGLh0VpVTKC%2BrZx0MK4lbWV%2B2bzz2VpB7xZi2quL9%2BUsF6vs92HIP2DhLqaApfLksVH3DZrsrM%2FCvtQcPeoAzGGAZ%2BdcuZOcy%2FNhiMpzS7lLpF6Eajpm9BlQrfjB3S3kpN11jxdRqgW4bYoL32dxHOgqu6FZLdQQD0SFgW4KlqVobD&X-Amz-Signature=7d5435dc9090b261538c2fe1eabd5822e106d8385917318a9e4623242c0e4b1d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

