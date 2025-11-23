---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SKPAF6EM%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T010039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJHMEUCIQCciAv59mcTxFk5dUv6jJKyJa6jVekr%2B721PN4iskijvgIgS1OknThCM8g79JUtNUohrhstz7U86BHsi7droaz2cfwq%2FwMIMRAAGgw2Mzc0MjMxODM4MDUiDIg7Ma2s43U8dIGVrSrcA7QCQV4RlKaahyUQyJxB%2B%2B5dyZQ9QpIco9PGGjb59509tC0WD2WIAAHzkc%2BS1r1O0%2BDaAL%2B%2FNFCDn248EI5dRgVI%2Bm0EQB9nRhSYivtpC%2FPoALwFJONadeqIDykh2Y%2FQyWZtJfCzXIfRJpbqcBmUW0zGGWLWmX8zOrGZJ6oNRc7tqh4K1Pbh%2FEs%2Bjjz59QaGH4SJX%2FNRQjQqbC88qXQI1JGVnDl5s1f28zTQQY2pNj%2FsP13B6I0EyP70Hw6f%2BF05FPA0aykxaPBEhF9kzdZXrUMzF7DMHcKIBRYrt04MKdOrAOIblVBhy4PwAZFGUoj6brzII2H%2BkgqWebzf2Mnyve68ym6ARLbQ2PGV22G1jMi4QGcPANAogkifJ%2BJWhTFJSvVS0ykYG48SJsUAzVdkqEZwNOzQUmrstF5p17kcQrG%2FVp3EnfvU1sMVIlaKQVgEURi%2BLX5n34bBvsurKsRgDAF5Z91tMfyW3rHEiV8DQJprSwHA436XXKP3sxPjEPytwrsmtwpe9P3Uj%2BESZkNC2A%2F8ph%2FaMItUVPvLIUVHx6aoxKwLbrWkYCkSPHzXWW3lATgQQnai5PjoVM5gtFMokoFbUBb83T7Zr5jNgH6aVUQu03Y00e1AJ2j8REbSMKOfickGOqUBhNTx9fBerM1B0dPpQarrnYxp9ZmaJU3ufrq1cKMUGvNj14v13STOKpi6EtwwdKs2AaAKRozoBVVsaZoWJ7nLR2R4dDn%2Fubwlrds1bxFoG9uuH%2Fzph2e7PK%2F8SwTalqI8aGNHBSJnqE%2F1ysAfgE3ZyjllhG%2F7879F%2F9H5WlAwpf1h%2FDD4sNGvTkrw%2FEMUZiPQxp2NoZLl6ZA8hECS52rN7qnEjKgx&X-Amz-Signature=55da7c5a78e178964a1d333b71918cabb9425a39e9611dfa1ac90daee72e6fe9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

