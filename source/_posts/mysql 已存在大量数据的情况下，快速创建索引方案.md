---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667K75WGMX%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T070054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG4aCXVzLXdlc3QtMiJGMEQCIG6UZ7H%2FT9QekSQjY6rg%2FFQ%2FlBAnZaOgbLykqltit2jGAiBB2kZFDPdIlIOGZCkC309KB0RuuIcsclZGPVVojSqt4iqIBAjn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2FfXJ6uorrSpyKhysKtwDF4IX16dhcK9RK9ce56QyXSlzPj58mzIPQFIEk8RW5gjIbvqIZOsajLTar%2BdMfc9Trno2Z6%2F1NzOEa8v88dG1JfQJzwR%2BxyXm2paoBOGvU1YxvZSV8G6eYiLoMa5%2FC%2B2uzt%2BLVrTScljvpT%2BJunNJhBieaSDOFTIouk63On4mTiwHA81JB%2F6%2F15d8qvvL5l6YCkwbFTWFSkiW2U5SC8b07TAlNv70US3TFbR3z%2BYbSnywyZIv4bX8h%2FNeD3YjLuUEoWZkhQNR%2FFY1s%2BPVzbjiZE7tqvIaNCm0NzrFjK86cauzdRnzR%2B%2BegX9EQFblNd6c8Rve%2BKjBnrMTZfpoKYrkomhwLl8dArckvEg%2FsGlQn3iLw8Cwyno0eykH1ADKDh0fRy5Gqv7HYxvklNyLFxFMoRZYoJrVsHGzWic8hu8E%2BrPS%2FpqtYIocRZOkoIEvkdJNFlUHedwsm%2FKJnKTFioufvgrQwS9aIaw%2FH3gFs2JdBIsetTdh0tIJq2ORydSTSb1VMqpx0PARlKIpea77ab%2FCEj2oJTIX98gHb%2FbaexfOJ5ykgc0wimJgSL0lm4pYkTXuP36YypiHBOjbw9H%2FJ8F%2FS7I5qJSw3eVj6uSmkc2QZ5a8th82FBB%2Bj2mhM1owtIa5xgY6pgGKoFpYdBonDOvUcUeWGih2LWF4N7lzkAqHyquaZJYI9CpuNDtyoC%2Fh8DoUGSq2V00dzv2Wfe14TyeZNKmG1NViLS8MhnzOCxY6DNaxHg%2F4wMZVjX%2FJlbusQgp83s%2FwccDBA1j907Va7MDQ%2Bowa69Bq%2BeXzw1x81GTXmGYZsCC176Bk7s5vT1eBp0%2BXN%2BCrMff%2B8nWeukK5cNEm6jTtJoxMGIMC0Svi&X-Amz-Signature=a90ce22aea19abd58121f39a3aa3d41a05347985cf9c7a7420cc8b5b3337e8b0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

