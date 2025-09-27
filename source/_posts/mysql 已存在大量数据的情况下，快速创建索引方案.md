---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662LQHG2MT%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T140055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJHMEUCIB8CWHCGCI69Q%2Bzz0WYAQXHtKaL6yFyZsJCEf%2F8GGemKAiEA%2Bb2booxqFsq%2BERexx0kLEeMI2hoH3IE7wndF9e9uWfUqiAQIo%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBwCp2hRnCkI8AEhjCrcA87R6WYyxAKPs6kAAgwndSLq0%2BToP4WQggU5%2BR3wygAFiNoZ24TCHMW1xaChOzkrsil5l4565FNlzdpwKSNQYOxJGE1xd7NLEW7h%2Fb3BdJZp18Eu4y2QTeHNLKMch%2BXx%2FTcoELda3%2FMbu3c%2FvBgw44Ic%2BesczllJ4yXUaGAy97%2F9QOQWO7OMtxJbamrdmaE%2Bi0GoVWBi%2BSSslnNESDCXbdW5pqOBu4TnUWUUiAFdxn41tucuzYuvTNbBUegyT7k4rq9tIk2xHYEiijK0RC3uhqf8mDn%2F%2Fin6nZssM4Zrsj3yUSYokevRBLr0Lrx0cg24Q7IpaL7uzm6mZCdW5YftVXv6uZoWgm5a0Mb8cHwVuMhvTuUWSaWDtoc3NrS5XVzsZJtf3A3WUaP3re%2F53o86AOzQXEfIgli7Ujk91PiQSn07Vpkzyrum6a%2BhphYDvLX0FltVfmDTVwgdLiY5gmp7HW4CxYQ9LM9ev8oFpSFXbeUQl4rAArhRWOtMX9H3xbDR4sGhf4jsAft3ap7T24KlJGRkDTHk4uxIjEoiJaVQdQMu4OOIrr5%2FefGdrsInAUj2B6QwjgimY8C94z7YIzXSWNUDc1k%2BMwURgdSX7nRHrW36hRsiDhuZKXtoZlmVMIPj3sYGOqUBSBg46bNE2TeJOLLA%2B6V8qUe9p7a8YBacjkt3d5u439dfSBAVJN8GDoNHF%2FTcP2XXXKV6NKkkO8G%2FHAf7Un58hY2tvPNFiiEuEy8UgEhqCA%2BiS69Sw2eHwNj%2BDl5kX3A6KGvasDQG%2Bs80MYa03NuhgQZk6uBGySgbt5QP0W187K6yW4JxmEJqt5bGwRAyUzDziLXiE5ASW8heUtIyf0la7lk%2FjEKh&X-Amz-Signature=b265b109d1a28bd34015693983584a6184d5ae9b873d901115f1d2e41991b03f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

