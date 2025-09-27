---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666YKS7H2J%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T010041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBAaCXVzLXdlc3QtMiJHMEUCIQD8%2B5DaO6qs8gRrPhu9607vYfbOT3TeaLKQRbsqFER5FAIgTvTb2eOnoPCs4ZBdowZ0uI5aXYZRGqG4%2ByG40m4iSTIqiAQImf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBs9RdtWB%2BDIyICc2CrcA49H3pFeLCGJrDeNC0cJgXetAm4GklFGK8chYu09Q%2FmFh1nHHWAkbHF4H1ezMQjPtdU8obMpWlEzfIcikhdMv8bQ3rGukBYqs%2Ffs74rznv%2FQvarBjTuGoUq%2FiBfLl55w1YlrcHrHMwA8zq%2BzNmkoqyKdTKdM%2BGGSzsBXWmo85zCzH2fD0Tqv%2FIUHIdoaqvnCu497LHeNKVZ1c%2BGNrgF%2BM2XXNDORXWyDTVdLPkH0Mxa2aK9RppZ5Zq%2F6PIcLmbORJ8PyHpif5Lj9%2B1Qw7%2FbEwNSaes5E72znE883nXW%2BiUGEoPiSR%2BXzsgSHKAO6y2yddJYyGWS5fs5vmB8INTBNgWmktFHt1DyUWGHRLayGoYkZ92X4h2l6%2BRFWHKqMgU2DhdV4evs6UK9zfX%2Bky06kIoXuqe%2BcpRESb5M8AU9avVw%2FEvhR4KCoKkoJQv9GF8tkccJ2ElK9aY5xs6UacL8Yh%2FeJjjgrQ9Z2P1Br04WdgcRtsjwV%2B5X6rSrFXy7fbYdSiZ4K%2BiUBuDLp11uZxZnwr0jjSvPQ%2F3HvCxdJslPKlsAB0kAR2uTcpamkfckjcDU2HVUTLvbkYbsKgiYwzTULSsynJTM%2FqqC1gihWOwx%2Fyj8CVvTmMOLzTG2nvH6GMLfR3MYGOqUB7gcPu3GF%2BmwYSSezVlQ0KxEBBjcbrFfDBId0%2By%2FVvd7csM6feE5FLcbIchdbzIArl5M4idlpJSK8Qg19Ah0vjZpJyi0UZRFE0RAWGUR8VvYvBCb3TqRfXQe7wEzWfQ3pJWxp5SqLj6VRHQCL9TuS41gd86DmT6oi%2F%2FOV4RZoCfZdBT77SsRLbkuAi9jxrSp%2BKxX%2BtVSMV5g8Wa5s%2BlTCU0d3bViH&X-Amz-Signature=45fde6fe5707e056431bd92207998271a321f27d1f3bfae4fddb5a9e7d2171df&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

