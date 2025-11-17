---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663KVPKOUF%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T130049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDIpOz%2FAgf3PEaj7mExwaPwInGkAC5nhJ6KyYhkrFY2XQIgNo6c%2BMiAkP4bqQJOavTGVtDV3GdJJtWRNZa0NA3gwTMqiAQIrv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOT0zDhMszvUhjaJxircAxCfXmy1EfHthQARuKEjtnit9V0j1vXUHjnucP790M3DRgmGbbBF7u6AG9bqDk9rZ1uBWdgxMQLhdk%2BUYmESFGY4zkgy72RIdz1P989Gpzs9pNr40BpGFPZeQXbLXUvQhb5SWWLdqIMdd2%2FatvkwrO4JqrRZA4rrxcXSWpD4ru1vR4s8XZfDo%2BY%2Fp1U7RVoHdY42edU3xHWl1INz86Jcw%2B9bDG%2BJifhJUDF%2F6v4zFggwohA4bYxTeTVjFkB%2FUBh0GUvjLhzdvgPYMLF9G6y3VkCXrTTNiUxIfTvP4R%2Bz%2FjhSF%2FBN%2FzsnM0Nbkv0XcXitM4bTnouvMMMzNQFXqsp0Ja1eETprbExWaIGy5DDRyYB9s08yy3klGDWYHD%2Bqz6LEdMdQYLe%2FqO8853Zpi2AfA9K7Dq%2FCxnOG27ph7zrSvJSX2BKLvA0Ak0w%2BgzCOOYVl8Yebu1rDXaUrCPO4S6keNWIEy0CUbfmY2au0aWcoOSvjyQXAEzSI2kNa%2BKtwczm3MyJrEZq5cqITjTvRAxvWJQ4Kr%2BNZcG%2FMhAyU26pI0UWSyQxn3QfoebSyFM5mRrF8SI5mXYmWByoxbHDnPqVJyQCQacwz51mJxeFQ3A8txjhkRi%2FoL8hRH8n3JaQEMKmu7MgGOqUBuuNBmAVI4C2YaPO4F5JdYuay6VbuvaRfa6KyRavCNk0zzIGCNR6X8qg3xilWJG61THBNVuk2PG5oXAscEgRqnxB4QdMyqAcaY36GA%2BsIMB57DFDIFT1lvEo4QfxvSfHKA%2B0j0J54hPVWbmL8WUy40xSGZv%2Bi5%2FTqsXrtAln5kgWFmeTKw57aUvv1kACG8j7GMCG5hpbmPCK73QqNLO9FzuzQCYrI&X-Amz-Signature=6bb2498a3e5ae8172070133baa82902679e45187b395c09347db12aa00466d1b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

