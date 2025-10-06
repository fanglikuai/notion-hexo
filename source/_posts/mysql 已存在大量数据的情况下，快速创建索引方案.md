---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667IQ5BJZ3%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T150048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDXbtsJDuLQwiy2%2FRaKDmGap4L0yMDm2%2FJAz1BLR4ISygIgDd%2FQi%2FQ%2BVhFmifNR1h%2FyagK2W7z6%2Fuh5VoQjgOIGobwqiAQIi%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBy5myaPYGqLs%2BIF7ircA4roR54tHvo2DB64uaubpR7%2B9zIt3v8kf91Ls%2BLA7e68%2B06DCKzynhcF7D5zhPLvunplNekGVt%2FhnuL2ml80C6nqUZy2subRr5mSdfzPtfp%2BZMZkbj%2BdkWzqF8gY1zIz7kfBvtzUDwSXQcYvsNaqifEVJa1zQghEvtSAsPkSVzsNB%2BPZ71c8G9C%2FWXQRSzSG4Rv9vXdln7yfDBooPMu7fVd8ng10VVj1OdZj1QoA4qD38wWmk1ez%2BZTUgRbbssUMwF3yNfxLAajydCWjbfOSi12Z0DcllMeJAI6jQu%2BHmp5sPlxH8JePtvCiELK1U7CZFOBbPC12Kgk%2BhArILF0P%2FX16ntHNwVUQ66IueB8nWC%2Bo%2F0w5BwTKgvOWTgO1u89UsYyGmm1wnnNOFCbxRdB%2FQIRJaFSmRHDtYHKQSWnPgCcK7TayLs82rKgtiYI8vIh8q3LzrKnG48g8ravVB03yFs8VoVdF9KEdkOrXAJ9WM%2BLzMrdGszbA%2ByXH4Z3Ms8I53b%2Bq8BiRxE0q6DDTwOhXHQFFcSpud3ESc6AtLzm7qLkBCMK%2FKym%2BEdDF0pj%2FJSzvdATxHDdSBG%2FTNeheyqiwlT%2BR57wjS8DdR5yd1eH0AAATvL01uINZd9NflwjFMPmvjscGOqUBmCz9yI4wXmWx%2FfaKRdfxR%2B4d2V16yMtywA5v8TVdyMeY%2BbHtcw8EyqG7gA9Z5eWVO5ktePjld7TJEVGGMv4dOPHaSECw3fgCUZT6ef85nF4Htbk%2FohrIXwkN8%2FLTqoCufmXSjKcLtXUyONPO95ID8a4s1zpndwdASV3FYyZsw8q3L%2BsQwauKXWvhSJtYv1SDRWgv0qVWTrXe8pS6B6L5NiedQDoc&X-Amz-Signature=c90ec845ba3a3c3bf24200f1b73067e2abb2ffb7704e1338d1a3bc3e0de7c661&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

