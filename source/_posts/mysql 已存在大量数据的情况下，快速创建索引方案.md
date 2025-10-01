---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RKJGPLYW%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T190120Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCgLgICgXbLXdNOfN8x2LkPsHNwVnvhUVKzpWGOOciNkAIgdcU0rVrP2ARnDiIqBXy%2Bi5vioWdEnYUuEToghAIcO5cq%2FwMIHBAAGgw2Mzc0MjMxODM4MDUiDCINwoOZ%2BTpKl8xovircAwg5La4Vh7igvfF2sohx8Cu4xeMmLPnAzOEww6S%2Bwe4ACM7KuLITi8tDoUzBQj6A%2BUTJOYcp9jme9mf7xHCuEIATlc%2FuCPnM4TGMTidJp%2FCsFfR4PGZSrSYY6L2n3RurICXIcVwtC3UtzEucGhQLsAQOtlHXmZZMfOJBhG0rqt25wa%2BDlp4teIvJDi9CjCCYwN0VaI3V0wIsFDFh8WmQ2RcTcByYBI4IN0OoT9H2%2Fv1pDoBlps9YfJilX5gAYoaAuHRQOHPBPMOGlPCm0aHMhvHfMpzwS2nHSVSa6bBKIOQBu5FRhO%2BKGAHKLDk8KxiZGK%2B1fNEWsypzUqGei%2FhE6PeqqOqVt7eJnXjgAOS5oUDttJnzMXPsymCP%2F%2B7zJ85dp%2FPcebQlku5jjIqgiMGcBDC3LQQ87BrMUgUAVkP6uUmWfiT6PDWfU5QQGAGbd1tMro%2FVanSR3sdVv13j2eNYPF4YTBBXufzMHqyWxEIGduZEjUT2uVQ0pNtIVccmKjVJXpIL2fRE8xuXEzMrhN%2FDS2KoEnuROXGKfDQzOACCTrr3PPTDNfyhkonYNmUq%2F58hTKIIhG4%2FpVX8b9jUGBGHhErkMAuiUPmx4Xq6ZJ%2Bx1ie2kCzlONB9PMiujEvcMJvz9cYGOqUBwE7XcdF%2BM4XPrLgmwlazcOx11CbOxV%2Fh%2Fd%2BbT9xCB0lW4htNyteKQXv520E0X5eIY%2FXkRHOHgfl%2Fg6w3Nep%2BAGoIA%2BG2xmminhp4aJr%2Bm1rMMVR5YmWBTMiLrTARwTJouFf7ed9VLPhf0b5fEMKiIE9PILdqN8dA3r6JhWYwNHdI41GBn1Y9zpBiHUHiY6ggGG2u3P8kc2OWoJLjjXemncfq84mI&X-Amz-Signature=572d19363ae03ac8261dac6507489b7db4d2c9c7c061782ad75e61217d33ba87&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

