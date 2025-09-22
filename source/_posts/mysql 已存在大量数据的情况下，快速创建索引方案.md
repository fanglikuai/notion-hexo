---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XFUTZONS%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T030044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCEM7TFJEc3xPydEo0u%2FmhhSewUt5ey7VZsXMuoMH6snQIgSNp7D5YMn8llp8ibsa%2Fuy%2FHeEKJOViovGTh8MNzHLqgq%2FwMIIhAAGgw2Mzc0MjMxODM4MDUiDF317Kcy2JSgFf3FNyrcA1GBaxGF7RRsC6oiQDofn08Xbrkac3LbDhEkgbw7R8%2B29buAY6jRGkorTXUIxDDouLt1RQmG77PGrI4raBXV9YTMPyRFp7FQEGNOBOF95hGOstuYmemctaZ0%2Fc7HyXlrDfiS5HuwUm23RrZ%2Bs5la0ecKK2Ha6FMJ3G%2Ba19o%2FqV1w3u9DTlhHbaSJcaZDBxa73QQAC4xlomx%2FkzbaZ0okOScW9cwsuOIO12ONMSvjji%2BtlLkwVXracBFiVK0zsAyg3nr55i5Mx5ENwg9XfnUHjqQi7MWxAe3X1Yk0t4J4LAONr7t0AYq9Gnl5YzKSV7SA4NSIohynl4RYCxudOMUyDWWHOawclGAi6l1u0KC%2F40ZQkM1AhYCxfcF82uPaYlweJigDNbikl%2FeZ7WeKvRbDclG9%2Fe0wrgMDGprHiTeKmLtb7FSEimEA15pIR4XJ1P5IjZHsMEhRCyrtkdhspgxL3PHKLMvZpMh8nGGnEncfCXIPg2C86h%2Fgm3SX0ORHujbjXy4aoPbsv0pFa1gfg6xtH%2FaCNkoHLiZHrIkxgQJVV%2FlidfUqKwCxrLke%2FFhJ%2FR3TJiA8B9Yp888ilVFxYx%2FXmStv8UYbvJnfngw2Kiju3uO8VjhAfQQ4J18ScTNnMMKwwsYGOqUBavTQU61VIyseqgFfBhwUhgAjv%2FS%2Byv%2BFlGSED2SvuP8QSrSGGNWCAVfy%2FENsCmooN4NTf1BLc4IPalWR9loNi%2BWFDMSP4%2F9lQZspP5zmMCbf1z%2FULLHdVdU1PFwDpMy8pNsHthmJtUtAAKZ25wfmNWo7RrUfMigftT0e1NgD8OQmBzxvXWijZiRcSD7RKpxFxbfTEiVHczxrylJqv9huHjhdhOcK&X-Amz-Signature=d96a5e72ba88fcd1836b3754d89e81d1219d5b365f43e3b92fce8bcda692c73d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

