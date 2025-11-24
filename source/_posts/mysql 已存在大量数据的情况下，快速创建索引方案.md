---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SOAO6SSB%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T110041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFR7tMqMnwA7M0YdOOj8a4XpyOtFdCBN15xyOEfvWzHoAiAfRqfNoLN9QkGlVldbutNFDHl6bYNLEPNPFn5p71msvSr%2FAwhUEAAaDDYzNzQyMzE4MzgwNSIM4tG4PQgMnUYs4CY2KtwDvBIz9NvOANP4ncToc0UX7Pmo5SPyRBcJsNsFrhvHNvLtH7%2BWY4c7KOksAGVxgp7CAIcqmhf9wnWvA7V9H0LKs00C8euF2UAewDo6X0DveMJ%2BuHD1%2FUYYC2cahVYsgExOEpFwJxHtdponnO%2FpYjELBFgMxLEUwTiteb%2BTQjKQyDjmemPa38Utq9ROD0XcU5nV72YohbtQsDjTCrFH51tgIhe1q2d9INpOYpqlckpcqqZHilTdH9XW5%2Fk7E8Z03BObftDLPvQVGRaScKOBYpGa5QyWVDfp5nXV9uCzKneY%2F1ubnlvi6ehE2OK4ML6azVG7Q5fFOI8zLjoWkg1KrSwHI8ehy2fYpmm3aJGD9YHrixbstafCJzV8xrDZh91wfP7RzW5NoSARAKLO4XcBmW%2BlV6zh9w4RSHoX9cPtJuf33SCs3eLP%2BeWvxN9qJpSFn1VH1iHt53pOke62PdgSniirHHrWudKRc%2BJPZlm3nzrxrxUfpV6jYLNgQX0%2B4mOdccGrLXQG8hmFZwv8UrutlwKTaxexWmIvoL8bzvpdZB%2FtSPltp4BSPGXl09KQDzzBacW2M3h0%2FX1yH4Az3ivSDUKLldg3Pyvs2DH6ih7tamXF5h5nUjPjfHAwQ6W5Z7EwrOmQyQY6pgGRvF0Mom3xm9F6c8%2FV56jmbk7cdeJGH2lspIdJ6zHp5bz8rsxw%2Fhyv%2BXp%2BPIFnhK9VtxVcyXPD%2BphuA0TgeQcpNX6FdUmaUoTjySftuB4ITJUdwExJFX9%2B6zRcHAyL2keFrvJfhXTs7AzQ0R9ND3f1kRHRtcExsx%2FO3a0tF%2FosC9cvsU%2B5%2FRxASEdqE4td6tVVm2i7l%2FVAyPFDChh6ZDs14TnQY7td&X-Amz-Signature=2ae0b7635994a4944a95c9ba286a1b6f8e90887f4c50460e578a0c991b6618eb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

