---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YYBM3BZA%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T160055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDpJW%2FEnss0iZ6%2FzKAdCV2Sc3AMZ3MxhAbD%2BZh26liMBAiBOtchsq9rxZMiM0mqaw%2BQ37%2BZ5NLgj0d5zR1dSdIVQYir%2FAwhIEAAaDDYzNzQyMzE4MzgwNSIMzZrge9MWKopTeua3KtwDIpiFhjtfLtt5GqwOjPqkZQTFg7qyxwIeKte%2FM1OP6%2BLRBJzPrGYyzFB8vCS1bF%2BLCCOHO47SYeIGE771NGaEEpNgiIQjZ%2BoAtBqBz%2FCYjoZT5YdJakcxyEUiwOmjhLbggop4opX9wI0momYnVpBgrIgNagsnhtxmJd4aus%2Fa26A8VonMPHm4ZYJ%2BCpx%2Fx%2BKwuj6a1sHmwl%2FNESfrzb6GIq3%2FvsqlvEWQuACPLsUhmuUmhq2ivU7HtbuI4KPodPVzb0%2FtSTwuiT2BfEvaCbA2vLt5R844zZsILRHcG88G89AA4DeHYmmpHgp3xzy06MZTyNKiowxSTVPLDkGS1px0XBtsp5notl2LCl3qIzs5nIZfd1%2FRECU0oPr2DQU75V5yqUXy3%2B3UTSsPo8cHGapAW%2B%2FX8t22%2FX%2FT7kBRyqgnZaZZAk8afms7OizDx3GLTXnRJxTNVOOqa7EJbM1RbQfrTA2s0%2BXLkVCOZxro3ontTHz10WH5lOA39Xy%2BJANUyPbpXwlWbTHzCc2jZtWAg8dzzpdAGpk%2FWib460ZrisFEO3iiJ3pqvFPnm5Z30wjDuIigSzbG0NTiAJj1Bj4dP4pTAvBTuOy16yd5k%2BpyxASpWAekItp%2B8iIPle19t2owtbG0xwY6pgH0vMjpW591ColU%2BqeVKn0oKUoW7mXnghGiSL%2BxB0FBBD%2BDtb3uFPPS9QwdlwG7eJ5LRI7RvhtEioGyMba%2FkTgwBuTudXbgMj2jQ%2Flye9%2BYXf9EM5WrfGEMsm1Q0KOrC5gdmolPOzQ2ZkqiztT1l8QF49LoDdSKOi%2BgJ9Sg2UlSKHS8MOS3aHfAwT3Jw1dU6MfYLvvEAPAgUO0pgCiN6HcF8JoaQr%2B0&X-Amz-Signature=a307a4bc8c908ee2d1e40e306f59a7817c17f40e98eefcd98f9dabd264bdfd47&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

