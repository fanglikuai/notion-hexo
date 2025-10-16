---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XCLKWJWB%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T010054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDj7mFiNCdL1yEoSikNhLhF2G0YmOuEDcCDxoluihWQbAiBKCIjY25oZUdhOoA8E8Ecq%2FprykYh6v1gmmfUzcPwNFCqIBAiC%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMjJGlAb%2BDcFEzEtarKtwDNwIsCO%2FMP5T9jcMQBbCINPTMpcnzLPybTV21C3i7wN7QtLLlX%2Bwf%2FhbAQGEBQT%2FGCFp%2Fe1co3IrGRiEK%2F3Kt2hLNLgNylHY5CdGUgh%2BlLQzi8vMQJT6yij47enYWETYgg1%2F3n5bLhJdZGIaD9%2FEn9qb5JuT%2FwpkrXFprfGk1VMmEIAfXrOwfgFzEZQWT60s2BJeKTw2MAO480nmojxKBiIcuH%2Bucn8XcRTsSjep83PyT7%2BzMsTZNVfBDmY%2FiM6%2BTsgZoMdAy%2BuABdBYahHCOQmw9y%2BiHuQa3ckUHmDDgwusqKeAVUhaeF0Lts1mYz8mfHtfMGACrRVM3i3uhFIECu4yQjSdh7%2F2qAf3l7gx3M2770KgxJPLZpW48yAp7CMWi%2FxLEhJyXJ7OGCiI9Fq4m0%2BumofWQqF9XBXa8TopOfX8Au8i3%2F7xfQWyrV7gYRJ2ZKoRRjKLCjlpIwA1oMA3e5%2BW877FSGsDuBh%2FIHWbHZaywKpRxJyWYSQBGAbHOsLRo7patT0SnqcgzemuMOBjOj5lbKjtanQmWvZuhDyddJWLgCGcQl4IKND0xYgOotm5x4GJu06WlrtsXLXjeKeVwyNemcWyxYxX3io%2F0uqj1GT6PIb5qefVp9VECr5kwwvrAxwY6pgHOE7czl7wIUbsX1CAHxJP2Nh44KQXD9loeHd%2FJFiEtNZ5zbSLxq1iHstxV0Ejsd%2FVgAKsYTlNPRf8B%2Fk%2FV2eDQhJINMkoo1Le8FP%2BOC%2FbPpNmJV7MUg2SddzKzlW7MEwfYAbZbvZ0SVgjS7DWir80fr%2FMmv1QCE6zujQX%2FiWbcv47LBlRsB9gLDN15Mu881xrcc8XSyEZZ3mj8s0WOIJGpqiu6UZ5J&X-Amz-Signature=b1c6145393d6d3f98f46092ab3fd04644f14c51a2f7ca73f41b91c7045bd420f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

