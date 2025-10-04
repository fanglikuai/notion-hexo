---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZY7OX6NL%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T030039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDrnzEATG9VfxwANfsVdM82NA%2FkjzUbWpMwEkfDB50SPwIgGuIUYktxdFUF2V%2BaD1jRPO%2Fi3VM03INCB9TsoX64xNQq%2FwMIUxAAGgw2Mzc0MjMxODM4MDUiDNbWlealqu3jSoyLLyrcA4WjlcFUKSbAEHYuFbRBifo3EabJjW0L8yl9hTqp3NYJ8I1TCPYBg1aCqTePNhD5JnaaMy0Q7Op79kXH64RDKJiVP%2BdKWsvjholGsrxBv0pGV4Wb%2F2S5OG2UtVsALaH%2BHGvcC4JEChbnUH7qfnxoJmhaNChAAV4g2akf21fpCZzqGrdm8KrJuyPw%2Bv3HYyNNxf9np0FkdOyRB%2B%2FI0DazHC0ZNzCAm1CSL%2Fx81rUwJlWuUPGlKeF8PbZ0gbXWRW5WR8LX9bgPyMAkjCiaZRgQLw0q%2FO%2BX0z26D28%2FRErIq0KHfIpICmjFtsLR0qgZ327oB6easn6P%2F65dGb6dpxWSIbVojo%2FncVXBIJXv%2F4W1tWXCgic2dCh6db37EHSDFzK5Acsz4btvapIw4fP3vU4kLHUg%2FasV%2FUdRwcxsD2mc5YUuA7Hru7%2B4JpCEFONvNdhl6d8%2FChjb0kMIoim3DoiKOCNrFU9h%2BJ4TrFdRTD8VZQ%2FTKKtP5RvsTMT4%2Bcg1JjieTp8xdI78w6VslikUREQaTFJod9kjtEqv4RLPh1MErSnlafROPG5gYooVu7tF5SyOc6e6%2BbjaJ750eNNFoxtCg5RlKfqcpu6Je5qpBlEWhSbxTabCeQxGYHEhHamAMPb%2FgccGOqUBhpuERkZO2ZbPi%2FMitA3R%2FVWkr7DGBuCiG9TOZB7M9jIa1hMOs0oi%2FPMLQDoiutSUR%2Flc2qBN2mC3rs7VPb3c1hAqD4vqtaavFyqpD0or7OYlNf2FgB3QggXhdPX04NsdtsZKsve%2BnUHTpjyOF%2FDLHaeLBhZxTRerrI9D%2BabLoLEhhBiXMm%2BW3SnYNS4MFBKgAdA1l7ZAO8h11VXZxSjsf96WGyUt&X-Amz-Signature=76e433bca5cc554571e5af9f8f11e290414222f6b552642f12501bc56a3d91a7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

