---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666FJSMTAR%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T170038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIB6KopfUNGgDGz6jzcuk0G2OAQo6qgX9CdeMlsJ%2FzyTrAiBDXQaTTSsh6BPQlXd4ZPGy%2FdzRjqevsRg3I7cjRTlJDCqIBAih%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMB6QS%2B8jvmr6%2BR2KGKtwDv%2FYfGiyMgLSDSBfO9UG68rBMHL12F6Blz9bOfL5hFGiTbLBlxAnmPm5dkGcOaOtmpxbjBJVEiTAEXpjqekOlyJxl8ETIww2VxiuM8%2FA8BqoN4E5JglWxaJYWwQpfLhEuzgcYAx%2FVZ1IBqkwOtYzf%2B8D0xOtZKF9USA3OOkiEZibKTgAQDXCvokDicwyxkQahdtMoiNMMZPpLLRt78gKc%2F2rS%2Bew9bS5vhKlGaJcn2XZ%2FkBvGhh%2FdGwBxwxP%2Brx2a8dbrOEJXHt6pseC8M9HoVLZpLGXlqyrLKjM%2FJD3dxehYQMBf9D48NO5OgLqYBejGfGqSOF4cCrJG4ychM3gSLEgs0Il8ndfvkWdoboTS%2FMXgCqRcJW0hILsBf1m6Bcz7wI9Oytj%2FxDu7MYqDXxG6OKb9w%2BRPbeiyo3aDDlnEsGnyCF3TEoa1sVJyw5HB815fId%2B%2FNxifM2rJ7H9yKl3Nd8DN5lWlFPGJjenVbDYq94WpcZuS3x%2BBuufGCO2NbmyAd%2Fl7aijJjXVyWNyYC4RHhB7o9eRngEIVOgoR7fbASgZTXcL0Q7wEv55oQakT1NsDISvvimJXPpketnSAUBb5XIVXq3NuXZiQHsTiGRrvAgp8Esb4Zz2h%2BGMRxosw8e%2BhyQY6pgGCzUmQ1wm83wMVf6ovMqtxBYrhjcDSqexXcZYK9UlUHCeBCcOpZOg5EiBkE5q0R5SV6%2BQ9mqfP0A7LtZSD%2F41g5U%2Fp4LFR31xh07zVhENjQCovq8J1iiMc%2BuWtDuH32fxqFPLBpqcmDWcDTZ2yHQfohpkawcz0S2Rtf%2BHNjMy7J3NQZtAwbK0u%2Fvr48mmTcvipuu2p%2FotUfudAwYccmFtYraTyYYPT&X-Amz-Signature=6a8fdc85c3bf9c6ddb9ac79bd443828a92140acdf55a40e61b5a735ad4b03fa7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

