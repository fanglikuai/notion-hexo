---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662JNY2CNN%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T000051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCWsFCUQGzAHguwgdTqWGtf%2F6sPeggUENhOCRfWeDhRmwIhAOvP35mY5XfybPP2j%2BNd4VRGYwhnatj%2FmeOtZ%2FxxzOQQKogECJf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyGoUVOeDkQkrd16fwq3AMC3sd7n6ynJ02Jy11c0HZrTWmBQkLvqxnGFpIXJNz2wZF8FTWJ%2FTn00FnmUx%2FsvIjqXI6%2Fr4SSAVOcomo3GEaIVr8srkorWnG%2BM0Bh3CLBu8SGilwrsCG%2Bd8rezAInaF9u9E1MQRrk6nekw7AQt6d8FtkKyo2RJ1DLZwR%2FnZXiWwd87ExiYd1TFxX17u1ItHHfCE3h3gQqgCU4cplxbHDfpiex9GMKHY36%2BpYe3jiXldi1aOtLaV4TV7Rhfm6YRFnz82db0TdoSKWQSPLBphpn%2BAOARHHvL0PRzGlMCEAZZg22ln%2BvM9HgEkH%2FkvxEm6%2F3ZpH8bABcFuYCiyYu4HvHWHYuUXcfehre8Jze0DwanfUvX7yOMJ1yq0nNsVGdA%2BJirmMosiShwLGUxJWYaIZp%2BLbSsGNzJRRzkJnxWl4KDXHJpmbldP5YZ%2BjEP5mZwioyL%2BU8bizSPEBUBaofdQZoMGpaveZ3fzJNnZzG%2FwPjqeA464q5pWgMHCrc8n1O6P8x6MIHcvkKCeD%2FZ6GHmHgRvQMRL0P43gQfGsKwJw3quKSE0CF%2Fi5qCmv5ye3rYGlEXqamAQg%2BH5lLwe1Ic9coU2NlrW2qh%2BCJ5V%2BWyGfQ41BJB2c3qoMZbr8yjXjC79ZDHBjqkAX0nNrM6ufb3bc6BOII%2BMwCx%2BO5AQ1jivdh8Jas8GvW%2BfWHtmLcfjLjJG4WeSY5DGkkVOre8XD3gPG6ufdu59n886CzfglTaZ4nKO%2BcFrZvfhhe%2Bm5HnOXMBYMXUBiWirV6fvJ55688EvISw9qGAMtccbvpm2khcmviEt%2B6J3JEMq0l6PWs4iUCZbo6ee7qI4DSRWEus%2FlI1lyKTNKqhuYNj2dMX&X-Amz-Signature=83fc25c027adafe214a8fc32a18f2cfc162c89f3514091bace929dc815eaa81e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

