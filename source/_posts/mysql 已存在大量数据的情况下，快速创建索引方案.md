---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666PASSHC2%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T030041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGaL12%2FQ0Eepd0nNK8FsxAiBJTRWPZ6JEfxHmRqP1aT6AiEA6XVUlbHym0vwXxDBXisSuJFYHnp2WS1SNopw1WNuywAq%2FwMIaxAAGgw2Mzc0MjMxODM4MDUiDKCsujg035IWROuSiSrcAwr12%2FaeVx%2FaR%2BEp1xuSdctW3vej7pwX72O3bXkaPqzhvHAXpnB3e05berOftSzqP9AiLbigB67x8DV%2BVg198XBtWIks%2BwkKhR2QpxRJuf0ZV7MawdP5XM9P9qdkHNOmEc26L%2FAtfYkQOW0zPFSwmMSRL57jGVDmLn1Im4wbSsP5dtH8eZCZqObhvLGE0u8uLIdWT%2F3e7suDjquT7GiWVGJctO1xWKy5UDBBLr%2F%2Fy6PmK7ufZsr31izJYkjtri2CkVw0BEoBn3fUBMw5y5lfI75pQ1T15NpkHrF0D7LMBJRSKeJm2EnyVFfrAwUshqRGSDeFgb%2B0mOsXmmf0pBxtbIc9EX0qr3PgdIMTaNkyNyxy6IccigNYaKCaHEMZnRv68tGmKs6LJsPujOipu5xKeVkeLL9sGdzI8uSYKz59Uq%2BH5uILkKcLJ6mcUT%2F8UTsdm2Cihb0r8a%2F%2BGLx6qtvLU%2FtSadXvKuV8ZC1BJKflFFKM1Wakvd2vFUWAsW0fFNzFCN4hy6b5qwgR7Gtf%2BiMTZ8%2FkaoZBlZIZn%2BrkZaOthnBkZvhnKTOj6qX43s%2FtfUL2XYuLEL1QIjpE1XItywqkIkMGzF6mmI9hsAhgqOv84n0KMa85dbs1zn%2F3HherMMTCpcgGOqUB8KtVEr9j%2BivoXzhlfDrPJumTbSRSxpMPtpQRrRfrszxDVGDucr0kDKPqY%2FLoLBTR49pKR7Ilm68MR5xQ%2BueJqk7czJQC4XahS8BfRn9i7DNcTN36eUaBBfJeCVZo810ay0JG4u%2Bg7pGwT21Uy35nomtqb4Mp8%2B8cfWWMXyfEAk%2FdLbsfSbAE%2BvvNCDCxQRk2UA99wfUnF%2BF8uTsbsf4m%2Fd6iTxzr&X-Amz-Signature=74d1f2209694ee3ddbdfdbb25dd72014c62213e2233b92b7762c21eb4487cece&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

