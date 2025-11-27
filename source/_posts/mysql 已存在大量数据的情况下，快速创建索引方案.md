---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WDOUUZTH%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T090047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCB8jEHz%2BVKmrYd%2FeYPbS7zKF%2BZc%2BqrqtszoegyB7p4VAIgC%2FRaymRR%2BzGk5J2%2FxgDgujvdxiLV6ixtz%2FxJfJNBrjMqiAQImv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJLFeOlWwtx50h7lzircAxJx7sRgCWM2YyxulzXR16YBCg6HyXNaRrFy865aQbvwv99ps%2FWAwrXZR0GvV6ASwA42zQh58g8WNkLp%2FdzwI%2FBICHzLV801UJeUR1S%2FEqmenkvZxYlu%2FOmRf86aIwfkOl4Lf%2FZHC%2FwzZT1DB5TPpCD6pBdMydDO8%2FjhMyC5w%2FC3imeHQINnRiy7SJ5esF6hsvfHmggfiCSTKWeSv4HX14KB1p5HdWkWo0HbDpDe2a%2F1VRDm4Jnr8Sex68DLX8g2o%2FF4gFSTVSzy8O0z8O42gPBo1%2FdUd5y5lhGIGE%2Fo7yI8I2gPfcZYqlsRWqTa9cOoAgSRQ82yCYPhkT6wF7VddKAiQo38KPRcGrVlW%2BjIdGJdOyrdjGp3Z4IV4lto1C3yJP7lnKyaqo5S4F%2FZCX1TahndC0Ia4OKJThUcNqI%2BaP%2FDNOA67lHjGX6EQiv0BkbpeY3amrBAkekl5qclX4kACe48KASMnc5OTLVzufngdn3Y%2Bdk7exVeWMlUla6X2Azhqj3mQ%2BHpFGAf8ji9SQLi7aKhR6bj4L14Gvg2ki2Q%2FysQchWaMpycUGFaVWO6LzrAn1B4pfS%2FVNBEA4AH93C4VNsF4rdXZuLi4yKehz1ZohGk0Wn%2BgxCaIjrz%2BRcAMOGioMkGOqUBqJt1zm8s7ll6qaORfG3LegVtIiLEy19P6boR6Jp65B00Y09KcY0YIz0BzWaC9OW3FyhSkh06imSyDhDprEx2sPiFRelb4%2FtlSUaoDvv2E2DY2OTM4uTgyureqd5vtU%2FTUCc%2FaNM8SdJeD%2F%2FCeY5yAemBgHbb74lFR%2B6mQIY4JW5MgPFh2ydZtAt%2FWmJv9NUC%2FuXyosfBzOG%2BuHQ66wC7Lpdkpfnx&X-Amz-Signature=289df0d50387fbad89cf90778a4d1ba2a87c0aa7d7ddc32a90c64d1e27b36fdc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

