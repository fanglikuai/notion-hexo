---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662IA6XS5B%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T120053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDl9OVmrV8aUdN%2FKCQuqBfUA4M%2FpSqRTg9TN5VezADgcAIgHKJFiTP%2B%2FtctxoZmx30%2BWcQTHBzbvHwf3q%2FryzkOtdAq%2FwMIXRAAGgw2Mzc0MjMxODM4MDUiDNWUmBeESny1evIMtyrcA8MKxxd5sff0XZObdzt3sbzh16dhTbYMWrgV7sRJul%2F0YxVS5SlnghWWOY43rkEfoG%2FODPKrSUemjlC91BQGGwJ3gzvZVTi6df2alboc%2BqslE8mjz3jCh7rd83VVYGdvxzA8OpwVXPFeqa%2B57YuFMMf4KWyDlY3OwR%2BdQrpzLi3yaIqwEGQAb%2BsiwcPIUn9F4MJcDhJojh1d4lV8mf9Op%2FLjarl7ci6Q1%2FNlmkkF%2FLG4Pk57dL0JpI0n%2B0bb34jzxt1awkP5z3462naxjIsO%2BkGuPHg25dgvkKJfz6ZZz%2FOG5zK8h%2FlKXGhazSN3NPmtmMKOq%2FRzwTzBpbaqXLsoA8QVo0NeQl0PG%2BlwQoVO3JHQBwyy8aZk32koKzNI%2FphCjEA0U9aYqVasgB2n5X0mjuCHDOuXqoxNJRte5W4fr3aDpqov5XdO5uGV7m53GQwHL8QK6spxLXNxYHAfppX2MrtHGy49haB%2Flq908WvZfI08TwU8NrqvH7xPTXsz3g87LRYKtCiVIIylrP9mtKxsMzgdXvwgjOK2m2OT2EkyW7UkA0G5TmXF7u7JF9ihirm0%2Be4HzlI%2BvfvySJTQEGCAlN%2Fi8ieYBE70zL0Q7srdPFqwWdOXHt%2FkNkE6olp3MMfzuMcGOqUBVFMxPICSyyMzTdrWiHFTIV3zWhDIsWBZbe1qjn%2B5FTwivhFtWmE%2BlAtq6QpBri1nGftousA%2B9gsJSic49l7gGcdBpNs%2BaI7zPGlhLNzjGTNigMInQcIydfkzjQ2wZwCFHE1QoonSIWo4NouuIhEXnszP%2Bx4iPJ7FHF3Op9fuHuoGFuCbPZrks06wG7XH5lGrgpojmXD4EyAcKbxIaEZQjzr8zq%2F5&X-Amz-Signature=21ecfe8ad61fee4414a2af9146ec23309631dc3668fa4c3f3ccb534df1b2541d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

