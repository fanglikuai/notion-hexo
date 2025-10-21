---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SJHTKUBD%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T210253Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJIMEYCIQC4gq%2BjhilUiH1scIY1HT8x%2BZdKZNb7U1pxCYll%2BIP6EwIhAI6uX8L2iSIhxo9wzDKwXOdodb942pBUJ1L%2BfEJpmNEzKv8DCB0QABoMNjM3NDIzMTgzODA1Igwo4ShUMi%2BojPoGoPwq3AP3wxNywfGVl19G4ywZplrr9embDndBMqAJ%2FoMyYKTu21h7DYQXCua14hTjx7fNGv0S91%2FH4Cz%2BoOoh9sj0fRmAx5CWt5sCXSq3RVRhdojHjSOSSaRGf8fc8%2F2fdxLOpSChQz0S95X2RT%2FSQ1FvuwwF3pWHRuYR7ZEIWWF9O8DJiuEukI7SGLMlRh2URzBpTkwBLzt6fCQNcnTebDFMy6ZnDrwbUjAledKZAgtRLYwpNfOwPg2cJ8Ti6CgCuHrlUtGBqpNUOSC6iAZqIMTKeiknf0b9KAZ9u%2BAcmNTY1gYOM8U90uVwGmsEY1HUt%2BXr4oyR8vtBVweqEbnT5AyT%2BrfkdL2qVdIV2hC39%2BkMPzVQRW5nbU5FpsALmZcmB4dlFKgcpFrsH%2BIyMo8SbdHg1T6HZEKzXs%2FljSVHee%2BVdX0T2nG7szNJi5Sg5vZftY6pNYGdgozhE%2F%2Bj25Xul%2B1p7mW0nSlgatDVHzdppgwtjkbueSODP8XDqUDMOiH6maLIB0EflZrsmkvMcGEg7rXhFifvdaItqrYZ5KsVrv3MDTpqcPMe9k48EYhU4dJLOgmW0X03rrYKukrrFTXGgTZ%2BcfQfvq4glehdYXGJLp8clCG4Ub1FaIc6nB1wR28RZDDL09%2FHBjqkARWOlFJw%2FY%2FutAzsiazxiL%2FFSUELw2rgTCdqKUJtkGzzI3QscsTt0PVN4zZcLUrEPA9p%2BzS1hkDyCHEWsKVWPycOVwI%2F%2BOEe99VtEPgS2DJVxXFVa%2BAvRsHAZedLXpJOEfQgyHeVZ5VIINum%2Bw6SNifJ7%2FvWLH5jU3mgwjVzZz0Ln185gV08uzp77usxVO1fiTItzvPyDKyn3jhS6ULYm1CCgVEl&X-Amz-Signature=70e51300cf4dc65acaabf7c1b953a36a00a5f94c41d0a3f1c2374fafa79d9263&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

