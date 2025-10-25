---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UPUW5HD4%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T230044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBbbx41dz4DTkIneAGiVE5D9sWnhuIqHz7d5%2FGkli9QDAiEAkeHMZlPk8nbL6PQ1PtidQS6PbpDqBqp924MB57SB%2FNgq%2FwMIfxAAGgw2Mzc0MjMxODM4MDUiDFhUP%2FIHgsvHxSx7JircA12TXMPLaxFzLcjPZ2tYL8pDE6UKThZmgHl%2BUFoEpPP%2BYW3Dw2GZQwdk2VnCM9BEOShmDvfA%2FnJx8mURX28uG6XQ%2Faxl9PIGc32%2FuQG%2BXgE7wAWKE5h5b0UtNayW5SZvdRQ551UdYeO4XvR8KTO9nxD1uYJOv03ts6LCPp7%2BN4XTtU%2FFu3yGaWJ7vJE3JCnzWd5yFWRxmWjRqZkEvBih%2FjMnfmW8XsHfRvmQ64osYhW%2Bfhis%2FK8JJVkDRevZI%2BLKBkqigt5%2FKIL0Fat8ABIlu2qIrkITVOXeh23nIgSk3Dj%2FyGMQjBEfDKTvy9uhAifmvXrL%2FMTpLIDSoa7prKj%2BjpQq0qWjyJF0jiJTnTeLYU0u5cGC0VRgaAb26zjup1yg38AxJWTjIza%2BHkARMmfULmJkXPLnkH%2FrkCCE1FEvb8vx7erZ4ZKVC2ZmU3TFh2cJzp1YLTH8w0kE104sBJ%2F%2Fmy1oKgJJiwPM%2F%2Bp%2Fn5YimvV%2FCG4cVnqBxtb%2Bv%2F2KDnI0nOyMSzj3LfUDp7QCxWzC1YM%2Bj%2FHBvscfT9Rw8qLOoLgv4zEHuRa9UqqGvS%2B51ClMN74jNiToc8Soozl3o0LSBKaxicrDd8TUmYl%2FaC9%2Bsu87JNVCtFfdUsrOqtPzMMyX9ccGOqUBgsoMgZ1Ts%2FJ7KiCheArkvjwhsA6bs%2B1rY%2FHTqTjmir8UWOmUxzZqHyIX2a0qmRnGQzDAXPIuwDzIRTDfRpCTEdBHnLJGgqYAso%2BXcYhaoFHBtgixvYb9%2BdDyN7J4lhaurxNeY3emQaeHzvGYKIicTu9%2BjKFXQKXYUxSKRLk5YTRYpoS0a0A2b9OpaCLlOiwgjvwrzyfpqXgFivag1jERHqEB8LIZ&X-Amz-Signature=7265e9098297e3443420f3dc7ff589567ddbb4f12fea8273c9d280458e5576a5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

