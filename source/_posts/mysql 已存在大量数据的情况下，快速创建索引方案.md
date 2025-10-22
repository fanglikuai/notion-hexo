---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665QOO4YDR%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T230048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH8aCXVzLXdlc3QtMiJHMEUCIQCW8%2BSDHK6VPW%2BTt7c2hDkqvsxEZgJotc3cT22%2BfzvMrAIga6MJqOqHfyo0JvoiaaHSNuhlJXPtWrExbpI7X7PeP5Uq%2FwMIOBAAGgw2Mzc0MjMxODM4MDUiDPmrXfMer22NRGpmByrcAxM74MabOKtFANVti6xJeCcko57amjW%2BoRh0IpvzVvcQAbaSSKfa%2F%2BrmvZf2hPKdG%2Bx240R%2FZ57QcpZg4RZOEgAZOMDGDv1MT7PETdBkmo5LLKU7lipxiJuqMfY49RsjPA1UCSLtszO%2BpuLUNX7keUjdh3VvnXKgBBtVsUHtloqTTg1ZVgkvAxsDh0Elj0eyVGPkI3O%2Byd%2BjLU3COfGGBLy09%2FIOiJFEUuEq2K2zErUpsmyGaF8OKtXfbw%2B%2F6elqKrJawuJSWpwl4C4R88ba0Od6NcdQVu%2BcC9evgLZuPAge0gFPfXhhCDaDCIzYDyutGB2ysDtuba9PPI%2F4wh5DEImMe%2Bmu7F%2B3MPeT5mWEYqd3dQJMFcYeJt7Kd4iNJaIc7WgEkM5Fv4FTOdBX851B%2FAv1afnKC5muA1TqJ%2B48f0KOxYsznKLrGxSebo4pEN0nD9lIHTbre59t57bBcg%2BVmERcvmn9Bo0eYRQfecd5Uf%2FjVfSIs9%2FYdH%2FUgwi4MLx%2Bt8tlU2Y0kEyZ51E7Wn%2FrLVdlKrBLyRGLdxDru5fI8fVjFFAqxv7YJ4Mi7Mw5cx46ZooTv2fZ21YfPjt%2Fc%2BiOnMY9lPHPKaQhs7wE3go8kF2aeMB5ru7o8Pk%2FOEOBMMm65ccGOqUBLbcCtN0%2F0%2BUa5HzZWbFJJbtXbcf4ahhda4VZy%2BiGjAzu7XqDU2w4QpUmVdmlPVAHP09RKzPIvw%2Fj5w3ZzJJqFl30Aq6MAer44SyOwslvoSRtrHcSmCoS8utbLk1HvlZAGTf43GVRM3VAd8GBENdT%2FGHjflF%2Bv%2BRC48Zrr0l5QGUYAhzJwtkRcWfS2TKqRA%2FBTxNewons2OB5jSpEPM2dXzI4esw7&X-Amz-Signature=6caa7d0e7c9480fd594273433c2275d37e6809641df8f2232b18adc0d375703f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

