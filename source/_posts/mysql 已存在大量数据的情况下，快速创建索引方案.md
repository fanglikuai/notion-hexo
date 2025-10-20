---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666LPFOUEP%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T110038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED8aCXVzLXdlc3QtMiJHMEUCIHMHvp6WrLV%2BbYv2bNc19yCboIEXP6yG5IdizsEjenFyAiEA5mYsEAxqHnoFFE9bsBy2upTMvQx3npHl2PKnYyhjrxoqiAQI6P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGP%2F6sN%2BVdDT%2FhASXyrcA2%2FuD1T2xVY%2B4PO7GM5B6HvQfoERlX2WwZoVBUIEoqEm0SwbxHIgDtsuaLHnT7x0gxQr9iSeC1A2IuRJPV3l3u0K7b6zXwrhXkcdGl%2BhG5lMZXFb6voNHL6tePlRZ7hc2hOdxUvl246bvmLsFMfMbaP167oSks3QomKl1mFkCvSMQYQGaoDnu0pTsndzcv%2BfvtVVnEdWngr%2BTamnHPfOfe9vQog6xNUSN7zu1Q4kbtbBuOa%2Bxzs%2Fp2R6Snb5ZMpxtCo7%2FXI%2FtUdTVCNaiaRucBJkFFl%2FHWrvWX05SkPAL7nRQlMxL23smK3EVIuD7TSDByPgDMUGxlVgEyoZC9Tp7vnXc3xtg4EmD0hhWenU9S299qxJ4rrVXTL4uVR8PRqKoAK4urJSrlqEcBVni31pwkaQUiCEgfa1G2T7Kgs5p5cEYDjc%2BCalB7%2F%2FY9sUQUVqi3mjeBh42fHZI3JLD2jzUId5LdJ68%2FrntKl5gMjo%2FsDP9mcAJXnwvLiIS3J9W%2BE4w026cK5l83tpyP7eFJimyCIOQC8j6cLOERkGkz75XZ3GSKmPlDDPG7om%2Bsg%2BBGq3zSrEzHxmnU5AO4MAEnqGu%2BUeiMcvxsWTN2jprggX0lMldp41QwSmNp3SfG8aMLi318cGOqUB1RDS%2FygmhHM%2FZtVx1aGjvirrG5aTo3sXnZ1Ps0hjZB7uxrHh44yOnARRJYtUDO0kIdOYB3MBlFXjfFXg8r2fDtt0wiuq8BQYUf%2BPXrVt8A5zKRsM%2F4fgM8q4L5VZIxWHDI2wA9JbdFgnfrUNzPPEPN5uJqBAu8A1nC1%2Buv%2Bb5dsO1ABSh4%2BREgXfSTyU99QTbrKd78XhckejMyfijNaiqgOinm7A&X-Amz-Signature=e6517a0c06ef6f13756b35d9094f6cc70a29bb5dc24bcb7ecd625df3dbe43815&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

