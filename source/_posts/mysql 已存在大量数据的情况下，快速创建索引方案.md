---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W3FHF3OM%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T060040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIB%2BkmZTw9CpgsymY6muL2CNG%2FfUyBDs0lPfTP5y66LFXAiBP3duOqFKMNW1JAOxixKB2ZPFir9OckU%2FA13WMRvA9DCr%2FAwg%2FEAAaDDYzNzQyMzE4MzgwNSIM83Sj4krXfQygpj6PKtwDWUS24SkKChtblgeYvSnyNMDOeEYvPvofWf8gIC0ifofaySQfBQHzaayNBYY7xxmRfth18i4rJNtOPzer%2Fe%2B8na726R8GUpN664SsDXkDzSibleu5qdFnV48Jf4kUrHL%2FnvaSDa7dyAxXguM5l5Izd2DxYcQci1ZfCKSrf2gvUsM3gGPcHpUmdvUcPsqNhO1jV0iXu1QR4evXN45n2AT5s4mpsVpAC4PQ7Ic%2B3J%2BMAXx0Mi2MES%2Bm1HXp6DemUj38bm3Vayf7tLUd90nhpwpMpAoFECzKvqcHmHw0jSeBLfcCVhFw5yDrJlivpFCy2O4lfnD72oSx%2BdsSIgJsB1RNd3gLFERgWdPedjd6FrFUz1aJGqje0c2%2FRt7ipcSiPCtSPlK2RkunmZS2JPJVOk118vN7GaxF8IN4Vf4DaAfON711ktUUHAOLyfgDqhbXi1G2RvEaXoxbjnTSe0FtLuBZHJJO6r02Ml6RHdCqmrd21gxHsIjCDOJ8AH1EIp1LBS%2BmeNwN%2BZzd99sP7aqQJrwR1WI9twZ8bijJfO0mRrHBPsL8dB9je46G%2BYub6Mwbhbu3Npd2wO7zs5AP%2Fd1MWC%2FvTBFAUJvvdB9ZCzww2A%2B8%2FhzxxKDrJXlnbOc5PsMw%2BuzIxgY6pgH8GLnj%2FhN9HAO2tskd2ueV8nn7KWeRln6RpDAxDYUQCAFX1srDc9BvLBpPtNsew6%2BHUfavBkfw9foxSYpTFFAFsyl5aZb219UwKVqnQ1vh4fW3rHM3pPIWzzLrhU%2Brjs9oACjDifoukPI4k4rm23rV7eiAHqouVvsDH3LDzXXoGH9Vmu7Ex7MAAnQIFkNcKMTdw2bSXNb7%2FRT31kk04IlpE1qPKKKN&X-Amz-Signature=9f22bf719b0bcd32deb63aee2dfd76814f21c6a8b03aa409063f9b127f6c77ef&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

