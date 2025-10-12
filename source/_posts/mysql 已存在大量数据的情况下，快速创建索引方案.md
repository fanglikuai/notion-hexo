---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XMENVNYQ%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T000040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJHMEUCIQDjP1K6Pk7rVnbynBJE5dpEgfHiKxkXA4DswTtE2kZ2iwIgNyYF3lejb8hCGyy%2BNBG69qwRUvftM3q0V5bp4HFaA84q%2FwMIHxAAGgw2Mzc0MjMxODM4MDUiDDHDfZgGrhmy5I0I%2BCrcAwKGFuwjJxSkg2HHwEC4D9gGq8a%2Fo5b%2FvwWrlI5ta%2BOqnkL%2F238aSmbvq%2FgTG%2B5NwGOizG0w%2FxQ0Vhh1E%2BF7%2BqGuHxtBUE8aP2UbmzslnnjnILBdw5m%2FD8gL0OWy2GEJWmZDPNeSj94hQUbOUCvttAPEDkCHqxFdEe9whXaRvaH%2FIksYWCiI1LFZoHgrqJRsAZ4BhSg%2B%2Fik7hNGd3ozaHbpFvvz7IJzgNbUpoLo0LOFKHjbrh05YXm4xRhdt0AKVH%2BNy4VE0wTZwk6gyBH3vx4Gw30FNNV0Aj8N2wYzMa3xEydqoR7%2FW5Pa4EhLywiE1Rk6iYJCv10ZfuXPOpJhWIkFXB37jF9RITIBHX7f7rBcUB8V5A6DtVDYK9ehOKecaQuLsF1x6TuiDQG0ZfIGmfOeMjHYBpBnTaAGPOd80Ov%2B4gvsP0AjQcMaFavxtSVRCO011m5%2BiGKtdI72KKDMtBP7ii8VxkQlIZPRXZEfFv%2B9lZLKkkKQHg7GRFsqom0fIEaDng4vc%2BrMyvRr1eQax47tWA8ld8FAEq4XHSIr7d9QnAxapNuhfPD%2F4pkAF7%2BxPNIF5GBZys1SEnrNqW1tZTavED67e43agtymb4pbIBxijfMT%2BOR0dItWOJMc2MOemq8cGOqUBZ%2B0O1Glw75lCgCp%2B1dt1fRs5LzipMyUAG0kDvKXdHhCH6GAF3%2FucDmZj%2B0nPITfWebIwWTY7pbCC9BJz7HDq1I2Yjdb8l4GCInuhql%2B1VVYz8ldTh9vRD4lDv5wMvcD4oRMvQjAd%2B%2FSvomeTCTOJPeHkz%2FLMWy09t1YTQbmFmkGN%2FtOR5dlPQydUcMEDKNJHG%2BvrVC9U30B3ADdWLF6kWfPgvbeZ&X-Amz-Signature=1beaee46e5d807adf26ef5e509d17b52ce261ad262a122e8ec9dff39ee259e2a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

