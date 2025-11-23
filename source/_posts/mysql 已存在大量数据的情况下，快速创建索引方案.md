---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RQMVEMZT%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T180044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHcaCXVzLXdlc3QtMiJHMEUCIQC%2FrYdQlXlUBqQXWpD5pQUgxD8iIRvrtBXtftOPg5GjZAIgdDtZYGYpFAruCecN0OI89tXDOR6DU6YQuqVzGN7t3h8q%2FwMIQBAAGgw2Mzc0MjMxODM4MDUiDJTgSwqQUPIlEjAurSrcA6TNDi4uYvqJMuQ7hJcmljazrhMKvYmyCTALPOA3eZsmuS%2Fi1SvbWb5j7ZyFXVNZuUhd3D2i14nCWxtjfAs99U8rMabWWwk084HNGQTr9%2BfVb6UgKrBkwxx2QNOyZUDDcBwGPSfWeEBHPQCJGhCRahspjamo97NvH6UGuFQAKQj1DYza8BAL7kt82em%2FHf7uhuc9YYafPOGFRW2oPSWCr%2BzlzAyaun4vrYRc7jd%2FhOgnZM5FJwqOZXYM48IessDjzAWv92JidshOT5lVe%2BClZKRCd3rhlEfzm9vLG3WL9DrmyPmiw%2BUNSghJ0Mg1TzSMf2zsjw0JV7OMRxSGab9LSfnXybudBPQN%2FjTBKfOftR4lwQ7aIPZBtSjVq2LiDBZyfsi87LRV92DwwOU8iiphRjfPbCapQDqjZdTGrGRFuW84KGD%2B%2FgaxD3001vHnbv3enfTwdZwltXXxWfkwKtJeuGAaSOHZeV2lYO0E2n4hsUdTz6xoF%2BLIOqrGUM%2BKRsay%2F4iO3qcopkadp%2BiSpCXpsldTkS7bXQGKPoUJ3WYxDwD5yKs181FSlLSuZLPZP%2FRZepQTyqSZnyQQF%2Fpi6rO6HlgGDPqdZfAf3czUKY8gWHIwRX3JUUL5MHPR1fQyMMK7jMkGOqUBrd8953CX%2BFzBymoeuWXuN62kpFnRjXy2wnAsmX6ZAQw6lGNLyRTY9emztVq1ft3YEuoiqeTPL%2BJPu09n0Bi6ggWLaw1noDYVjERYVfi4JOO8R22cX0GRtdGmKxrTfahC5oJ6uYBx1ebmlVNYf5LlqdCYwFDvI0AsmZirJd5FiWeBw0MkyZA5h9p7ZFdT7wBule7q50huuqXMRJlFaXImahIO%2Be1q&X-Amz-Signature=680a0397a5a1c9d0c0e70c2ea3d79a9d70587638c8bab2c1527ad72d278f995d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

