---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46634JD4NXN%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T170059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGAaCXVzLXdlc3QtMiJHMEUCICiwbfm2eZmdu4xi3D7WWCnaXEhzIUtwotjXJysKVubAAiEAna1nxb5%2FVE93fRmHVk3Ap9rVQd5xECeJJyHgp9cmoKsq%2FwMIKRAAGgw2Mzc0MjMxODM4MDUiDIa4GuT5T%2FUtCoBQxCrcA1aU24UF0vWab81uO8fnMwpJWs7CTthrW0vXOmSq112Q7Fx5g47qfSILiJqYbyB9NubB682U7XEjZ74DkWTcvNfPYpfhzChmZW0eKeNN%2FCZ3ONgnlA95nsY9NeH6xS20kcidxoBlnZ4z%2BMkYova%2FaFjqlimfSbQOPW1AyKDXlKHx5sfunrxIMXjnficGz57jorZlodS1f0CaOsUxXBX2pgHA1DyM6HS0c4dcjocvN0AKdDHLi1woERua4suDxMtCQxeEFwnnLj9b%2FDL15YgeXta3ww%2BXLwf%2FvlXcbtXgQkn1l8VOJfkR%2Fg0%2B388igrzTNSaGbBZ3JdvL5bmGXJY2bWUhEqoswMAkO45gOLCP3EUAEhu2PP0dWSZVfYcERyXfUUATzvSNoXSzjtAJgu7RMGCG5sB3dEj1wUJf9vSvrk%2Btl5u51%2BowVPoaymHH2oudoOtwmV9%2FOimxUa%2FJ0dOCrKrxoO0xYdE%2FQLFq%2F8JKBKBD0PAtCIk1FGPV%2BUd2muO2QCtie5x4XFGK3Vp0tAZjszafssaTM9ATFWYYLOMYQk0X67%2BUFR9LRItB1JwK6M6HIXOBDF%2BJZDvacmXOT6t%2F5%2BzlSWRNjpmRx3iqUTBXWXuF4CLlUPWoNDxXENMeMLvDh8kGOqUBhibzYEnIjCypvaUYcbPxQAvu6HDhgJZhRfgRTAs1Av%2Fyt2bX5UzqfoYVFz9btWh0irTRkj0v6tHI1zG5C8wP4EZE7eKTc55056RNAq45bMNJ5RkGRFCjeBIF%2BQgl262FWSBjJc%2Fkn4la8ThZecI7i1hsit9FR3Cx0wsDCWAHWEhc4MC%2BMCOjyZz9lWS%2B9gvrsOC4kbl%2FZJdOydHgBPx8RSuq8MYy&X-Amz-Signature=63057d6d07c0808ce6e2fd47ec9171edd81e9efa1a4c8e3a137416d623cf9690&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

