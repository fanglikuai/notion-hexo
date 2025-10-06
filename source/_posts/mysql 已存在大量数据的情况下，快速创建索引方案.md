---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665ONBHBTY%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T160056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDSAwjw%2F%2F94sXx2ZCp56J4ybXqxb1T%2FO%2Fg%2FMztHvMwz5AIhALq0TuePwyiibJMgZFc4iTZnghuHKol3at2ZojvmosxxKogECI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igzer9%2FKxH1JllpQMuUq3AOjgPgpnLdNjB%2Fa873oTFSihV6ixxEJ2u80%2F9nOhWmniW%2Bc4wOf0Gghip2%2Bc19LjWxz%2FYiX5DTSchMKG7fUXrNCbiRxOyCnqwCRWsSJtD%2F%2Bar7IparAQigKZzAgyKWhQW4vF2IZ2nJyuChghUx%2FmigjQkjET9N1DAVIJSK5m9n8MPVaOVwrenSVdH7u9ErEidp1FFpkg7RUWqj3RU3izmwDqpg%2FgM7onRM5uMO9clFi8K5tWSY6BVRQtwMnaWj%2BfLlBwAybcwnvch3fcIVz%2FRUhbn8W7XVtWJOT4qfL%2BzI3%2FcMRcDbzAeXUR4hYwJ1g5WP1xW2mefQMeihl0WqlCEm3PzvDTCmAk6mkmqbjug2meQWnt6cMulGIe%2F5f6yIIRNWN56ns51DkNI91hDr6PnLsi%2FZgrfIK7yfx73TvPl1bolY9R2ShgppBqoqBLBtZwVG7Cf3tErOyPmR3Xjq5Qnda9ZdvTG3%2BhxXPgWTe5dta5iHc%2FDRdhUGEvXBf75mIimwT3DC1keOrPRDzSLxjzTcB5jB4wP6oyZqSTMAYNXjEsXgShXzGTf4N569Bem6tlZ3UTDtbb8245WDBknqNQOtydgRWnBj%2BPzNDcSnAcP461Uqc%2BOueXdtxFsbQxTCrmY%2FHBjqkAR8euD54zJaHlX2PGn2Ln%2B60fwrrtSNVUqwlHUGJQTcRmtz5Nhm0IGYWpJBd%2FHb4gapsdi8rh9iqVZD3yAUIslHlIB0ilgx69L2OZ8%2FX6WHLZHiUjJU30yULHIBWD1NmRefDmZaTf7KX5Ofp1TtThJrQtbobcfcvKrFgVEcymz1J29v3M6Je%2BNCAejppJomvsA%2Fr%2BmBE3K%2BzS79qqyVEkC4lnk44&X-Amz-Signature=5159b3891513545707cc7ae3d8b69597e8eb8201a5b082078239a35a382096e4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

