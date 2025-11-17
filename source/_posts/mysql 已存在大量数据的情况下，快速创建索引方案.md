---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663KVPKOUF%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T130049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDIpOz%2FAgf3PEaj7mExwaPwInGkAC5nhJ6KyYhkrFY2XQIgNo6c%2BMiAkP4bqQJOavTGVtDV3GdJJtWRNZa0NA3gwTMqiAQIrv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOT0zDhMszvUhjaJxircAxCfXmy1EfHthQARuKEjtnit9V0j1vXUHjnucP790M3DRgmGbbBF7u6AG9bqDk9rZ1uBWdgxMQLhdk%2BUYmESFGY4zkgy72RIdz1P989Gpzs9pNr40BpGFPZeQXbLXUvQhb5SWWLdqIMdd2%2FatvkwrO4JqrRZA4rrxcXSWpD4ru1vR4s8XZfDo%2BY%2Fp1U7RVoHdY42edU3xHWl1INz86Jcw%2B9bDG%2BJifhJUDF%2F6v4zFggwohA4bYxTeTVjFkB%2FUBh0GUvjLhzdvgPYMLF9G6y3VkCXrTTNiUxIfTvP4R%2Bz%2FjhSF%2FBN%2FzsnM0Nbkv0XcXitM4bTnouvMMMzNQFXqsp0Ja1eETprbExWaIGy5DDRyYB9s08yy3klGDWYHD%2Bqz6LEdMdQYLe%2FqO8853Zpi2AfA9K7Dq%2FCxnOG27ph7zrSvJSX2BKLvA0Ak0w%2BgzCOOYVl8Yebu1rDXaUrCPO4S6keNWIEy0CUbfmY2au0aWcoOSvjyQXAEzSI2kNa%2BKtwczm3MyJrEZq5cqITjTvRAxvWJQ4Kr%2BNZcG%2FMhAyU26pI0UWSyQxn3QfoebSyFM5mRrF8SI5mXYmWByoxbHDnPqVJyQCQacwz51mJxeFQ3A8txjhkRi%2FoL8hRH8n3JaQEMKmu7MgGOqUBuuNBmAVI4C2YaPO4F5JdYuay6VbuvaRfa6KyRavCNk0zzIGCNR6X8qg3xilWJG61THBNVuk2PG5oXAscEgRqnxB4QdMyqAcaY36GA%2BsIMB57DFDIFT1lvEo4QfxvSfHKA%2B0j0J54hPVWbmL8WUy40xSGZv%2Bi5%2FTqsXrtAln5kgWFmeTKw57aUvv1kACG8j7GMCG5hpbmPCK73QqNLO9FzuzQCYrI&X-Amz-Signature=03b17c009354e92bf7155f6fffdb513724b7dd2a9a058134df858fbbf77c05bc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

