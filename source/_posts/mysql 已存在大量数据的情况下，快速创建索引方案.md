---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TKFDOVZL%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T080052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBNvMK851TxZCAoHRCiFp0jFJak6g%2FXY3c8MdcgAYvHSAiEAxSjJ6%2BoXd8Ya8W0zMWJn6to1BGE8L3gP4WShqqbLbpkqiAQIl%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPgEnhApzMQ8bVUg1ircA5ApVTX4eLcYTtl9yiGV2lzytRTeLuVB2guybVgPGybPpAvjuDvyfJcRpQAm0P5%2FiylQ4kXQcUrbDpLOHl60RewrlfBLi57Xz90hfl1wo5h03X3ZSks21befhP5YRdzaogxX8UkZKs3F9W4mBDhuNgC%2F1Aj3DfpmnjhYmveQ6KorkUa%2FqZrwcyHJyD9fpvlH0bRy0738pDiTE9ZiEEk3pJfFzXtm%2FcqXscU%2B9c7Ss594%2BfyJFs6tADRMn%2BnHZeAwxbu0ReMWiK0mle3426TRBOKXljNcnvLa6RBMmUwrHbPHdVek%2F7iXnQ8Mpq49lkkAwnVOHf4iby4QT4SetCi%2F8sjWWOKsblE1OMtCq0QjJE0eFheT2nHO8jMEe5sg8%2FB28%2F0%2FhnarLIGachKj1hGe3JP34%2Bg1hzkLsp8A1Kne8sX8hZJ%2FM83hPWCofoQpACCk2YiPN1MdJgRoDy3fJO1z%2BJf80SBE4gLUJhGMzEUfmpAYvXUrOwIMhfEBqWsT5rh%2FzmEbe1hma4FqyTnuQ1yA6o4VzTDGSil7nYRmaI8a70KjoGrPE%2BRjhZ5g%2FNqhgjzNfLqvWAjyQwyloCsqL2J6HGHfeMIdnV40T6C4QHvTMXZsUWIO8nXgrf2Uj2sYML3Zn8kGOqUB%2BHPWEe47LCb9CF60OggtorHqQKHv1ug6%2FN2p17layCfsFt3yBSzzITCOr64qNnZ52SDNMmA3e6SAaHuoc0ljMyIFahMv1qgFnblslHSVOj8SV6IFVydNO3CpAt%2BlGasP8UEVGHK1DRK36ntn00n6ErBg0MHiGKQ5We63PsWIAilqza73WiiVlKLD6fujyE%2FQIu7HIV7fyX2No14OmJuVg9uFXIJe&X-Amz-Signature=5748ee809d9279544ec280489fa6d7e504ec1f3fe3b020b4fc6fd6ac188c982d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

