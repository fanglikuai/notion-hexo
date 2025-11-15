---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662ONPCSGH%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T050054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDfMOE2OKz4RlwVabLr1OP%2F%2BG1vnFkAsd3T9b2RQcdIXwIgEl9VL3SQxcqtZ7Yae1Enzlw%2F3uPUCW2g%2F8aKPg5hZwYq%2FwMIdhAAGgw2Mzc0MjMxODM4MDUiDAmRxRk3g68qRU2t9CrcA%2BVpMsnlbsdzh6FbVVNuzERkm8nY7hy%2FJ5y937l9VrtTAJGQ9TvuKkGlAj42iRuX4OUFbwyEOb2E%2FafSzzN456e9RnSpYj9w%2BKT%2BgG45t3p3ld37fMxMRJcBp4uakuZ8KleQUQ9RhSgfTilr0PiSyaSEGqT4uVqpFxOpKvgwIo%2FFo4ek2msCHzI3BZFDHKc5yFHVYXQ%2FZmdyvbZ2BRd1PuRjBvO0ntpums2p1aMVPUqDc%2B%2FJ7EbE6H%2Fb%2FhYnJjX53pjrSOm9Q6Mg9c0YYwJ5dDNdcR%2F7%2FxDJ3%2BcCBi0Rf%2BFwsjYHaGETSE61pnAx8BHIJm2jwvMAiQeIJP8ekFNB6jvVD%2Bi0HuH0hnIPqW91XPVS09Gtbc9eVYkkAtpKa%2F6ohu7Yqdu3gP1bQ2vo1ykvWUe5aVjOMrVibhdnD8fTYaAapJAYPfjgSPZEO1bIlXCNdEzCZG6AkJdDirJKtwbnmjNC84oJKlMooMHt%2BLLS8Dzr1s%2F34iW9hUa2AxduRAYv7ivgQ6D9jc0GDUVmf7TrTkaRRUnM4ShiKqPvi%2BI3QrqhQ2DqBmOiIBL%2BcLcl2wK8x7sb2aoRoazjJvNqbJA%2Blz9ff8f1cQms%2BSQVyI%2FhsfQ3d1M3fjJp2QKNqIcQMNOE4MgGOqUBVD1YxENr28LfrMm6VHhhKUO%2Fg0xuXIqRsedbd26qGyOzi77VY%2Fq4qEl2%2BK3ZctcEicj7h7HMZV8xPnQZ8geQ86emdPW8PNXkQBxKcXnOZdZLy5buNDipcSOYX05NOlYOZZBv0CvzcFxFXb9sAPNlKi%2Ba2D1QgBexEPBnwqwnXXSYxijrVtGXD28oEhVAN%2Buh0Vh1OjoJwu8EMgkzf36uAvBd6i%2By&X-Amz-Signature=8123e505f8d3ac910fe114aabce50375625e5e70213da74bf5838620a7bf0130&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

