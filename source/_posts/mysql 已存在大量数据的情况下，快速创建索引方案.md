---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QGM43ZL5%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T110057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHIaCXVzLXdlc3QtMiJHMEUCIGjElHzX%2BGEzbMtusYCehh1GWneE59H3PlN3kzxFuZHqAiEAm0iy8oZqMZfITR16Ue9TENAmhj31ZAD6J55bxSpPVsUq%2FwMIKxAAGgw2Mzc0MjMxODM4MDUiDPNo2X94h%2FUPfton7yrcA91%2BkMYVj1mJ53eckqusNt2gWfRyo0w7A6bxvZOq6hsHri%2FCPJWyQ%2Fnbmbwn8MIxglL3rtv5TiRj1P3ZQk7gbCHyxr10G8nplh5%2FEMEwEdDZl97EWyPemICnv2tQZZlqC446up%2FZxVo8QUe33GgvQQghoisnTxCiLxqHVLqISH74jYWAupwcF8SjP4ZjJvpdVH%2Baz1AuzpIE1tBhixbKZ4rsf%2Fxc6cR5XNQXivhGNFRM7w%2B8peITL225Ku22PlcChypppO8rJJfy05Bsv%2BSOgH%2Fc9RO5Pel5x95fc345Hbhk9t%2BLR%2BVQ1I4aDKBs3zK6LvhBj7KPW5tieKLni3M7JKY93%2Fr5g%2BixL2GhR%2FzTOTUEtvAFeIpLLVHz%2FrCfNYoVg%2BYlDKvPuGmb3bwDr5wn8F6aF2260dechEOnnYzY8IRAKkHFzyuXtpY%2Ba3sljsTdy2KIehtRvZa%2BmkekFH4%2FTjIGLUbpOERDmjOntOMHfbXkNZ1J0PMZtLI62Z0M2sa6hpD%2B2RgdrLSC1J7uNsebnwsEInwQY67CQ0OlP7913vOmnThQE7FaMP3JqnLS3zhsGDqgTfDN492yyHs8OLYBWLsSx4ITlJ6XftXGGb7N0jTenegU69hPwyNFd14vMNDb4scGOqUBq6rehitC%2B55jShRz2hsFJmP0dyJUP9ddvQM%2F6KcGxSp%2FNeV%2FPulh6iu%2FNPMafl%2BZ9eTHq1b6bnEpF1nnPAvBPqBDOvxjJ01x9pqT7829lpdMkdrm1uCmiTqqWuQ1KgTeV7jNhNYEXYZAE2K9G8peW%2B%2BQXTuD96azRWjdbdhfEwRwtv8%2FJsOy035iWicIGHeS%2BBlBh0gACzDlAKBGWyUBNUQqNn0g&X-Amz-Signature=d7f735a48a8772f919e88b285131a98f657be0282bdb93e0226b94a52f2697aa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

