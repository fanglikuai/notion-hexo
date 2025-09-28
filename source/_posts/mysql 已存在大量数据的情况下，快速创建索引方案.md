---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RXXHR2NJ%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T040043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECoaCXVzLXdlc3QtMiJHMEUCIFgRhKFlk5Q0gDx4D%2FPPU5%2FSJTkMc9DrItT9fVanR3pXAiEA2BDyTIEinANyqAAcE56shyrP%2BRT4MbNTlDbmIEVfr6EqiAQIsv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLwJMf1%2FT0TumRWE6yrcA2I9XoaPuNDypHOFDM4Q4jzEL4TCCDf5XSlWrM%2BX3UdxUrEb%2BrPgJO2%2BYXX1WIeT1h2wJTSjosT%2BbcNlxtv1yMas94CkguWi7hZ6edm%2B9grLLpEP5uyNDcyAK1XIyWS6PeMdMH2wTTWxW0URajllGosXAYr5lGeiYv57oj%2FLLNXo50%2BEiBLcdcUmBUitCdjzRGDT5YfJT17f6f9vooRDr10zYt3sifC1Sv5yEBwq3G%2BFUXfBNF%2BiGqfwN8Gi8jHyqzBB6i3jhMs9ac3oLYHlnQm8eeJhGuKLVZ%2BYJowA7HSmDsZld7EWIGYU5GMjWkEMEnpaIb%2B3Ul04cI%2BbLZoGr1m25lISbzUYuJxFGK4%2B4aniE9k4X8FHbkyFfaq8U4F14EkYbUSNmTNF1LO07COSXO3x4XrgeZuNLYUkD8Ayj69bjN8hDCg2Y2Chhm64MYcR0QRLtxM77Kgh3lyR94co1ZfTeJ0f1lfRqw04WVnQMMCavWC4CbooZp7Aa7%2Birfv5i1G%2FNEsiyttvxE2MfgHbx24UkmvWUoJiXoFJYzXdalJsKwUhMB1J6VGnyjAUX6R5yL2y0Oa%2F2UizIekypyRwta%2FntKCeg3Iij%2Fz0%2Fujx6h5zLU9%2BmSav2rFVrmlNMJua4sYGOqUBgma71E6KstftMR6HxZiix8%2BKScMDgqUQdcusAcbI8dC4yJDt8xie2guHKMIfoIf4sUT2mkVg6SAKitviHqk82eUlt4EW8DrAoy88zJRINy4kA8tvd%2FSaLDlQrcg%2BnIaAbR558S4z9sa3nZF1jjBl48HdtG3L%2BISJvjTkBJqvRoPjQHg225B2LKCBLJWm%2B%2FUjKXynC6z0eH%2Bs%2Fxh4Q%2FEByC9P9I1G&X-Amz-Signature=6dab01ab0ec1e0a4b99e4a53360ef24205669974f4d43ad8ac2fdcc94a0c9b81&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

