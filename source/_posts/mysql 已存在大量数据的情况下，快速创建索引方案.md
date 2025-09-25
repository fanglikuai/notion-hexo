---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QQOP3FUD%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T000045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCOugAc%2BPVDtqtU4yF3mrq2z%2F%2BkiB1fY%2F19kMlGTGgM6gIgE0cJx0bq6LXtvLIYxw64xtvAQlSOWDF1fVWiYOclvnUq%2FwMIaBAAGgw2Mzc0MjMxODM4MDUiDA0BFuPaUWo%2BBTTE8ircAyWrIRB2QvwYqyJ1l3qZi4ENekcD7saIwFURewWKi7Nom%2Bo1tUA9syFUZhXps3qtuslWqKqHr5oqRX4tDnuwXveBQn3s8EiKNJUk%2BNJYomnLu2mA49BVYhzm9Fi2AGHAUOEEL%2FPSwY5ScK3jexVTPV98hlOhCFlRkyz%2Buoqht5MLH6rrUXgLAGxGlP8W%2FPEp2ubl7GHw0lAhi4doGxF1n38woSRLf%2BjXVSrOr8UVXcQWjUb60nDRuJ%2BILTB5aA67VgFa05Ztlrhz9x%2F78b4RM98jCJN43crE43YL5llkOqiB3K7F6jaqdD2QSWY43GH%2BWUbt9MVxkT%2Fun13nPraNjKCjHdtJQOse7mU92b1oNArWvUDF35TGM65EEDlFOSm%2Fk5FEcTeBYw8hTMvyu20ECDiaWACgz1%2FF0JZ8glMbTE6sFnuMhVquhDD%2FOJd74m7XQShfdh3eSQUF%2FP1xfxGlnSPJJCogbFKNWstaXCo1%2BfpQFNbTC54DjFsTWBjWjFZwfN5p1qOCe71EL2Nqb5uPDlkz9ESQulBwHPwSWMUGXX5piPel0fO31NU5oeDZs2DJZAdlHpxZspjYQi18xbCfFgahK8PqdOvsNwU5tKtRIF4edAYNuDgamqikKa6nMObo0cYGOqUB0iYhLKfMEGTd1mCTxPtE75rbfuETB523Q%2F9PoUfbbLY1C0jhJxC0RZQ0QzohqY9oYXaKiMZ3G3udR60JUkdPfxja9XbSlHI5Q8OBGJ2u9e397GzLlx0Vwp88BQcMTTSMcDwlYDT89llHY0tx6CaDDJR6xNHgRV3GPyzSJFyxIvZpNF5wSZ2T1vB39pvE2g3t%2Fg6ZWqnFFlCD1Ti922uEu4cr%2BM32&X-Amz-Signature=8d736ec719b96fdd8e09e0684fbdf65030a5784097ac2d3e5e5f89dac9842949&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

