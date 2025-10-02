---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZBN7TV5G%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T230044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAi89HZTGGbnW8NQ3AkjQmqu9mLM969VXtBtQNVOIjLdAiEArkGu6ov6WBSkGwp89lz57IfkVUlAPcavvmz2u2D3iW4q%2FwMINxAAGgw2Mzc0MjMxODM4MDUiDL69kUpvf6yima1bnSrcA%2BjEY9tqlE3JKlaWZhWBeG3%2FO7zSOUJnYYLKhboh3Q2rwMf%2FQN2Hols1Zt2ZGLhTEmXFNoRW84YntUIOsbmYLFC2feD84c%2FgqYDko2zz3iV6db%2FzMNFA1Fq%2BJMJkgM0naTsRDUjMP4VKG0UryBbldxlBf7Ba4uRcgYFMxlWkFUnrPfqo7OwMYT4IZ4raEBWVza%2Fm5u1ZxMIVbpoR%2B80TkHrg1K5NRP2YgMcLZg4lprCrQDPWvfW79LIjaASV66YaQBRHlGtMnp7uUyAJ6q1K5F%2B3Rj7yrY56AaFfsnBGRWbJiswZIk3AKdRWjic5lze7rxnjQNeVkqbbadTlA%2FtffwZn5G68%2BPWKEzKUgZ8wbGW2FeR%2BE3Mthfx2MbsbRFb%2B93QCT2EyrPCkpsoaHjPdIsiMEjkvIX%2Fw5UJesW4Bxb1PBLCweQ2caep7Ql2HdBGlVPl450Kx9CROCvqbq9K2zQgrMZDsjDOZ%2BINNPdejywLoU4Rea7rs1j5EbxrcwDL9DRwlKcS2ZkXTW7CGXxeBNAPp1uQwsUc9KRD02i%2FWurdRG9K6knuluNR9%2B4Jk4OD8SP6g0LBbX4kSxA1q1%2BkWJLGzUV3%2F4Z%2BIL9HKwYcQGKnN9OERCupWOs1nJW27MPva%2B8YGOqUBD7KaDKZXcp%2ByKhqTMLlL%2BRAKRTohcIcvtSYJG172g3agclBHUvo8ZrhU8CNRzkJ7y1pd%2FAH0%2FMUVzdRoIjebkrk3coeir6AjScrRGCr8AyTyaDF2w8seT%2BjGCi%2FAqW8p3hFK6JaC0dukSPXTmxGspSu9OT6mqvlN%2Bmnk8A9zVxk2%2F7v%2FZ5RfLC5NzfnXkJz6ZoibQjv5tMmUBCyWTBSE7Idy8nxT&X-Amz-Signature=26b2fb338709d46221e4bcbabcd75abd86e1ae6424ac910ecea686803afc9328&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

