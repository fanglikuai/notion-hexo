---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZPI3PRL3%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T020038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDoaCXVzLXdlc3QtMiJHMEUCIQDC%2B14XTYx1me5CmllXDTZugsIwTZwl6vFNynld9mzdSQIgEpzivkhaQaS1PB3qXOItX9I%2F3nr1HRbJ%2Fk3GU0nf2Wcq%2FwMIAxAAGgw2Mzc0MjMxODM4MDUiDKbTMTG1njIHBL9JtyrcAxQCuqiKp5HGEfvRVNBXzeKhliR080QuZLty8aHg81ws3aBU0On1L47y6heoZ3qO4qCCbNb9doJGpNpu7A1ZT8XQNMvKE2Y0MmaKxSu%2BUzR2L0kSzlFv7KzseQZmSChfTOFJtJ7OPVGiik4cn%2BDsQfhw5m5gAig15eTMe9wTYxIqveAy%2FlSXMi128W0MPow3yQPDP5nI%2FMwkJ2wqH%2BMVpQU6lWzQva9vwWz6zJlEalNZC9fGPuJYbQ3XSQfuNKQ2okwy%2Bb%2BuH%2B%2FcePipYMvIDHapm%2Bz3o6u7bAFjPa7Q%2FoHBRr237r5tmx%2BoIRF5ctAjuz8tcwPAqm4GiaurG8vSH33PqnBSehr%2FRw70GJhxH6rSANb5zmFiTSdKH8hEp21JB%2FUyTZWcWa7cLiqK0tf8R%2B8dDjBbPgoO3Oh5DtMePSm2GafD4fmO6vs9SuyKx8lT4LxkAcJ5D5LJ%2FwLEA4orf3CVqL4MCslBA9YvTqTrm8zHNi%2FbOevsGtdTd3QloGwHE6S3P105X3k3Yn7fo9QKdSnkJgWLA6q%2FMv6Op4eIDc%2BrN4cAczhadhheWKYhQaDTJ1k6x6%2B0I%2Bh50%2FbjgXTFYuttpaxaAsDxNYH9uFClT0taGvdMkBVA1iqk%2FqV3MOyL%2F8gGOqUBpzXOmVakJwMaRAM5l8qSjGB1HG3JKvRig%2BomCo1W4bvuQMFNABFbCFQnGYdZmwfyqDdpw6ZAI9hzBPq9s5d5FHDNpPemiH5AlmjguKhuGt3CE4IKsoSLikkzuUpg5xGa1KqvbI4LlBSpJSDhcFe7%2F%2F6u9ZYhxKyI%2B8irMBeY8n2Ts2jiNuXDuTa0%2FEJ%2BgLaYuMQLjblWcd7%2Fd0D8ydFMLi1lz3NN&X-Amz-Signature=247fb345010a8d83f172b80e96bffc788070c878a4ee19c5fb869239a9c5e01d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

