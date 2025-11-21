---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666J4HTG7G%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE0aCXVzLXdlc3QtMiJHMEUCIQCgw2giCIrRCPNQsh6%2FwSKewqSd5ATutvJD82uuv9KZFQIgA5OD%2BFCXsUuH7fbFGU%2B90SDK50bLEGge2K2HRY3YwGcq%2FwMIFhAAGgw2Mzc0MjMxODM4MDUiDCnLFEv%2F6NxtspWddircA0Pt5eP5d5itub3XlNSA5sDZrnLjNrxnc2c10D66hY5k98Y3Gi%2FTXST%2BKgF2KealHMQpgnnXeqSv8bPJJzT4wdoBPhGFXB7c9%2Fdg763XEoS8ltTnLu3GjIRPXOp3UzDlkZ0cLt5YWHL1M0x7kz6gdftKr4lmg7P8ByKRk6V9RURGgTs1jSQxMR%2Frs7Kp1EOyEloLnWRsro%2FZKCo9qLA24emvqhtunufr4sqDgahT2fnpkDr6tMXSBwvSWoCXLTBG3QqbS5i7FPSbtvm%2FvFWlSo5u8y5Md2ZSRj9aJtYLgvU12rGsKiJwh7MlUu8tKM9FMitghIPjdnvSWESU3UB3%2FI6YTZxSSnlOqC6avbK6gVa5lAAV8JF3ht2w0Gl6EBv1b08B0MmFwwhg%2FfLfvBb%2F8blsvPqiFW3zaFXgJMt3%2FCsjDsa6qIk4HabE1hRvoN%2B79I1FbphKsp35mPkylos%2BDP4K4c4cj0qlhJqq%2BW%2FluyXlShziIE1FVWlUtW2RViTSpGHGuQ3RxJD%2BKFMpLbXP2OGpZEevrtUxw5B40h0Q1WEbkaQqUPGvmIG2a%2BnhmWxGy6oOeKvdAPm6Bddv%2Fx9wLuoUJLyL5R702agmVMnQ58aByME%2FrqVnACh%2FfGSVMP6mg8kGOqUBi7OjqEEYQOpHGstKA7vEIHfcFelbqgHpTEY8yPKj23pEa20ZsaQ32C%2FR3QzlywuaYN9lQhgHxvjX7vVBBaKPv%2FFAVJZNJuskVxWIAsyUFUQa7xBR3w3v6k0xmQyan2%2Bh5VWJksmUlxpC25Gqimkz4NoKnecH3aZB0Mc8Km%2BZjHQpsykjYZP0mx9sM3NAj6owB9ubOY2gdbNQ%2Fc4g4M5kW8EshntT&X-Amz-Signature=0f3f7786242f247c86f878ae2c2ffbfb0ef4e192317093b672b86e7a6c6df4aa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

