---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663VK472J2%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T100049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDnVAxjzQtxe%2Fj%2FJZtFD7U1ncu3ppI3yDvh2p%2FfdTbqkwIhAJ%2F8ZH6eBtYQnyPQJxihUdwB3jhQ2Ikyg99pr3LwrCrIKogECMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwAIatjgLrr%2FmBh0UIq3APlk16DeLqvRUkux63l3EujFbhO4ZsSr5KPWFy6BSB2z0L41H721h2cyMZfJw66glHz2tq0aKHKpS2Q7DKEbLkjrbenKpVtPGzhd5TTVu3CX5T6B6ne7xn4XnmUtRPiI859nd6hXIz7mF%2BdFNE2MZWzn8mAR32ywerhufCkHAmiYVwjSGoXbLsflogKEDaXSPidMIl2bMFKzhHj9hf14KxwM1rLC3QeAXNCMPY2rOPBYeNPxfK2pTKGMoX6x1cPH3l2gLfxi8bP4vn%2FXuQBtrOJzSywuN%2FGRUm13Vfz16Xlrb0YSnbB3F4GfuBrAiNsw0r%2FuE6XYoSSnDu34isOyvbi5F%2FX%2Bh9vT8Oz2P9wFrmyNvfBf4YJFl5ydq6%2B3RP8YcHQdZm0i%2FaNDjEkJr6nzM1eDPrMiVrqi68ePUxatH1sOVKgyk3iACD3lNk1PzB2d120Infg29rtQ91y%2BwdmXaqKRAbd5iDNW%2F21BteabpuczMQTbgkpvBmQqI1c7RQJ27Jr8H%2B0u2wpq%2FcPTG0qZdlbGH3Ztr7QwzGcLHevxJz2gbwEGg56ASDiLvtT%2F47H78sJzN4mEJjlEaPeSeqV5HfOu%2BIBfwyZKM6hDaAm5lmksOzJAX5q90XSZph28TCGhfHIBjqkAWaH29H9pweqjH%2Fih0kynaUvcW%2Fl1vwEDuQqa5GI8IPK4eOPIlvkv1o6mkQbC8WO%2Fzv%2BtnVj3tGR7O0UHV4NS6oh31%2B%2BLYauEqjuiQ4Jzvx5QqH7XiRNyXOysBpuI98U5ZDKvuVEA1TV%2FqHeiLAiNewXiChpodCYGEO9lt2Ixif%2FXig%2Fu9URlBvv5jTL5%2FX9VuTmUgOOX1ORLXecQGMKPKtAUble&X-Amz-Signature=9cc6bdd7ed4f15f06e2258dd7d528d35fa798d00f6811fc909a1d32d0d33a289&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

