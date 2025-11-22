---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667AGXEHS2%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T100050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFgaCXVzLXdlc3QtMiJGMEQCIEUDrShfwN8EHgX%2B768Ja%2BtnSzH78CWS0UJRKjsHcHgLAiBUpofk8E7pv2V%2B4Xud4B7ouKIFtSNUxqQ38ztV9aKUNyr%2FAwghEAAaDDYzNzQyMzE4MzgwNSIMZ1m3OK27cIz6AAw5KtwDX7eYGJi0ARzMuRPaMODG2eUd4E1Ge%2FyOxQOOHdS5NC%2BnqYBakz68aDrplQVLPSM%2BO3UdM3QwlERRRmm3PkmAWMLgsfx7pkzWfUPdQQZX%2FXub48hsBaHVs5rc2SmFs7kRxEd3KVCv3IbZ%2B9qhk6CexUV4Q%2BuGSVLsT%2FWkJbH75%2FXYtRgQr6KXyj8EBgCX1vGCs9zYJpTcc3aonZ2D4kgQu0Wr6d0D8u35mLKCEDX9sDcKWhewwQS7h7PWh33Us0gXUv15yOj0EzrVAZps%2Fz6NNGTBljgvJ7oRUthzmeqgbA9rA7%2FAjkMX76l74ObYql64PXHMG90VWzDGx%2BatLDTPcx38ZoancExspU%2FJRnIZgnzbeCMQzPTGZrxdFdJ8wswGJwLFC8ufqNR0YtwNVEbBqPNV62r4Ozjy8KsfXGAygtMgjG%2BLIFdaYBbR%2FmJyZDbOAE82HV32RuqQAYFVmvpYnXQm6FzY0m9DntUc9YPt1KESLl5cEOBVz4qeMyZNXIo%2F9mUDxQuwQOtu2cISkDDPhTVzeN52pfRJInLHjRT%2FCOnmyYoklHzTIlvB4EiYiomivLvRVw%2FfgSfwkMxX8P%2BWzh3bfdser%2FXT7NT%2BVxPy5V3lBPQ7t4Vu90s7D0Uwhd%2BFyQY6pgEYsQi73Kz5mc8DIG5fZK%2BMoEt4cN7XH7g3baxN0Lesf%2B%2FCIz2UdM5iFtyB%2B0OyOjhH05caFd49LWLs8AACJQBndMlutSRcKHh2GqJu4ZBlp3SjOgLQMHEAo3%2BKXrcWD0a%2F8VIVpYrdudek7OCjTU%2BdyfhuRGmRj3%2Fxtt0qGT7hvPs9ScLhCovXTlqgx%2Fn7uVdZYvHp05IZ8KON3GvOknMPi1vBlLOb&X-Amz-Signature=d71ff4852acd50a357b9bc0edc6b12881b2c8cda878bc7f8bbcfaa02d4adfdb9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

