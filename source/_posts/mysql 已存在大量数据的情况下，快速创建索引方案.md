---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TE66G5RS%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T110043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCAhm8RLrW9pldjIvBGuWfUZz3ojZU8tGAA09o7bwQ0QQIhAJ%2FQD4D2%2B0AlcoG%2Bc6kAtUOkeydri593g4vFrpSlFVN7Kv8DCHoQABoMNjM3NDIzMTgzODA1IgwNTFXtKvEqssW6a%2FAq3AP1ns3q%2FIYtSvz0KmzwBgu%2BV7gwwOJg4NHU%2FypbgaBHNc5oCBU2g3rO%2FHqOc%2B1m%2B94n6kedt9TptqJ470pNNbtocdqhMMuBQQlb1Cs9IEOMHqRisMShRULM62bsK8Lk2Zbc1kimQBORMuOfjRS3FHqYu0Rfzcm86bancI03YqBBh0v2j%2Fpc%2FgNCmJjaqSVQcA5%2BsDHadk2Pc5b6WH8b2XihkqNEJRySXa2iYo05MAp1m9y1gYFyfcjy6vmyT7x%2FpPm3RSloAKqTzus4An1Z7xmVzLazLQUFVWMOImDrBI2CZpYSmc70hgA4uTlXsJkwawF67LIMgQ5DRfYx0RQmhYLS6f1AA8fVCnmGRf5VovtuP54njNoUle2mn%2BQaBTf%2F40ktL%2Fk4QsLH%2FPqSmns9f%2BG78zTGDPH%2FDYcbuTvAkCIurOrX0tBSq4SYt15pCMPoUR7I8tzq0Y24dt6KGiFMYwIuCX7MuzOJR1ofQJv2xzb28xx0EMNfIWjd3iCuAqeXE7k9c9juAYXza5D8Vy%2F2%2BFJL3Y%2FSJizys9x63BPwnO0tWer%2BrYL6E1S3%2FTDAfUOUtxF0YVXLsC9yNlgClnMqAORf1HJbZrezloYGzitVocwPJS3MeZE6vJdVDaNc1zChg%2BHIBjqkAaT1LKNsIbi%2FKC%2F8%2F5058h%2BqigRi19DWq32V5yoOdpAfO8CdM7VfhmFUrxG%2Bn4PCZtETOt%2Fx%2BYDF4o20drclVHyf4VUIRo7j2Sui039ot5zQcbjUbG9t7l1Iu28RZ0Cwlj62o%2BKe5NAZC%2BTHLKb8wOSf9lN9%2F8zvo38YuV%2BUVA0xSNdmOZZsCeZhabVmcpSaAE5517Qxo9ZHMzfa%2BgAFxVDfwgyt&X-Amz-Signature=c70a971ddd58daa4dd3a594613e6b427395051bea299072a718139b40d9073f3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

