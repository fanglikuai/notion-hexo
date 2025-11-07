---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46673BQDHJ5%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T190047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD3npDN6epBgnIvEkDqQrapMxg%2F609B8kBIw1VLyRRqZAIgGNAGXUGGPoiFA81PnrlqWDdj1wugOpsX%2BeVuzy8aCRUqiAQIxP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIFwFU%2FRfzME%2Bdr6jCrcA23c6YtiBlJZd1rTMouvSzmWyc5wDPAL3jijWGhFFDusHDjnRe4AIp9AfWljPoOmnfOHbPyS5B5T8LRr3gFuFBx1HGolce%2Fsca2hy1VkCaLPlasYSTGEo8ZWz8RHafrsAcygFMFC%2B9sQGpbgYV9Lzm1GXMhvupNC7b4tMCrLF6%2FCvkc2m6zo%2FKBkb5YDXSS3tiwnD0fTsT8dn18DQw1%2FNwOImXPNWkoflvsX98GE2WC9qkrDeTDa77KGx2bPK%2FtHYLYI633ntKnLDHQmHUhW%2FH76sclaCr4I2XBthYUeXh0CGZzp77KZEnrKd7%2BLVg7qnyFK0l7x7PbFK7a3Mpm50lGsvmasdpqonEopMqsvWpY7qbemSkr9Uf6L9LjfrmZ9EF7fT5t0Ave52lZxtc4hZqvXL%2FKGIj8JYKQ4HMVX7hCuIiX6dU2RJSA%2FA2vSu3eMcWiTafYZP3aZPSF9UG05JpnIr9sDUPjdKB6VT3biJ0KImuEu8MlpSiAF811RA9BRPcREuVwDojyjxsD3ZV%2BOCXYEom%2B9q5gmmoR1kSoMg%2FK3Qr99STwjPYbJIvj5583UI03ikSSZaIr6nKoF54Y3lKP0%2BYAnDjoPExCXIaf1%2FAw%2FNotnTHGeT25pGW2lMKX9uMgGOqUB8%2F68T5qclWUJWc%2FKm4DyLUPv3ar8HddebZ85KpQg24KejR9WFKfs%2F9kYFtCUrSlzs2rbCZLv6aKZ8OYc%2BUNwYEElSc%2F1KP0z%2FWoL5gnneFFKlYvGelFzxa%2BD5EIuiAFdaN5QqNUmYo5oSRjgcpMZo02appm8DA0pcqlaK%2FelDc%2B1sLaHZMCaiwQBOHEaJfrLg%2FbTywaMW%2Fn9ZXi5O5ih8PpoMQCw&X-Amz-Signature=b2d87d5a1e1e468a860d3d17990fdcef1c5bde5dcd1f60beffffd5dc145270cd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

