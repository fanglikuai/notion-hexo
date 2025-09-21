---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z3XXVBYA%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T140057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFAYoWf%2BF09uUeGicoqnsxRf7dthGHF3pEmcdD2nJuu6AiB6n0FK%2FHN%2F6%2BOiTxxCkB%2Fvgx6w7S%2FnQsam1MsiJlWXTyr%2FAwgTEAAaDDYzNzQyMzE4MzgwNSIMVockYpFEOvER1csFKtwDjxSpf9SaVsCGvaD6j8oW0cBYKYNn%2BNbddjkWR%2F51tFyLKO8H15jiCkSb%2B5UfmOXtTedIr4hOM8ot1Z96wGNRxkMDuZvQxhYQoVZSYtTN%2FtimYU9V1ezSbHJwJ46MSl80MOzCzbK3rFe433mnp22BmxF1FOQhXdZ0rnjGH3UmnZqsVkPmS7RRrCrFN870e9uEwtbZYT7XQ1RQSVCqc68EcNoB82X1UnP%2B7ai2DQhJqDzkzSfk8mNqpqAtL6QsZr4zmhxBUOD%2F1ca2U3nCTcDnh2fZZ37lUr0xHkWQm2oq5XZhRtt%2FKe9yGsDYM%2F7cTYJiqILlp6MMdSxZde%2BC0EVL7lUCFoiWOugzAaO2bNyYZsB8uSIv5VSUkLp58RIBtc9SyyTLhrDI8x6inHRSmFxTw0I4t8WHG0U9vVfHRVDMdEL46NkQve2Bwlo9m73ltsOKvzTJk2clu2zVixjJVv%2FnbiHDHHCGQPuwOkKvLsCy2mb0yMRfFHP2CBx0tieXALohxm3IkbtYMJYIUBUzTuPrkJjB33oOyTtx%2Bp%2Be6dAqREfut%2Ff5VYRa5kpKaIaOgHlVokHQJ6l8n%2BEOMwa18W2Q7xFTxB0gEzjpJTexT2aLV4lzltSZwVjNdE%2B00eowsZ%2B%2FxgY6pgGPWRM3dht2b8w8ZIWhSHg%2F5E0VINGjOjdnjCGqRU6lLM2I1frZcbpwiAkf1GdBRLmnm4c53QypWJJGITZwjQf1rnlfcJ52x%2F1ulOJH4DPPe9n1f3pqqv3aQjL2PQWW9jrUYkgDD0RqfHBoVRYWM4iQVrCY%2FVlTLHbfsUlnFcZditG7M5wTU%2BQJ3O7iudh5KYtGuQdVr0uS03Nw1zbvQ8c73%2B%2BlXrEK&X-Amz-Signature=d339d10d82e733a8d0f97f310e6976d0eb4fe1ab8c8c2119bef25b986067942a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

