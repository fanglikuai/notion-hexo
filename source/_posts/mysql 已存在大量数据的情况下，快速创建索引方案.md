---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VEVZZBUT%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T220038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEUaCXVzLXdlc3QtMiJHMEUCIQDUECtOvawN%2FNG%2Bnrkweknp5iKY%2BX4ZF%2BKcGvlI0PsCIAIgQU1hkb4tVtLYY7ph%2FcAJNaGQINFTbOIWF8iseIEXnJ0qiAQI3v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNEqY%2FbKGQu7adOPyircA9nCBwi62jGWdUn07iabuzLCT0wKISAPq%2FBqM99FVTCSDFh4iXZKJvRxrJZdO0nnuUrREZijOe2X%2B%2BXmsKMX2c5O6oOevEwLZmp6z%2B%2BtCXDHC4Iqx%2B4WD%2B3%2Bspp3vZ8E%2BGPhsamsQh213qtlGOHpg%2BDVYne2P9cR61xsOUo%2Fa4saP%2FBY5ahluV7s560dsZ1ioe%2B2rmboEUp5M%2BOtPjFZcamGVtsCbTnr2vCcLxoVbyYhJM5UPmc%2BHqWDocRWMvoErBlygamppc0SANquLXq9TXWEyIO%2FEqtxY4sXWr66nHBWxy8U3rwBfaUhxiLaq%2BVYE85ocALf5xToACnx%2FZrKmWVz4jzkbdP1sxt82AfkNvtnnwqcz%2FPbepr%2BuURp%2BP%2BRtNSfOKtFog03jImsmEqTDCYi6ZwCXyyfxL0TidMX6dDT9n8Uc%2F%2F9Yw63M7Ar5lf7eLeG3z3tMXDWR8sI79Gh12%2Fx0XqRW1%2FHc5y%2FS%2Fl532TQP%2FUJ3a4KhphTib1Yck%2Fjew7qs4MQ3n%2BUt0BAnLqMtUBnm3MK3JDea%2FaQvKI7Mgz9KX9ILUlIgnAI3G2NKIYjNh%2BFp9tESBpiYbCX5cArK9fW3umFV4TYvqHPu6h6syETPRm840fUdxRMY3SdMOvFoMcGOqUBsSUZjtH9%2BnPffN9Z8HUUgZlGveIk%2B%2BORMr4SxIkX5i%2Fg%2B9drYozdwbvG3iZZFjqvw%2Fm77F15m6M0Wc1PG7NEMigWQVmPn62l1X9%2ByO4aar0MiaiRcvQ6wWBBYXn0Yn40t1yXD2%2FkdMI9DgURXEgr5BUP95NfOiewSFBNTqT%2FL6KTeQhmzLSzUEX%2FqHbMf0Ef630E%2BFyWxaDFqU92xGPe%2Fz9Tcky%2B&X-Amz-Signature=af120309c0ffafa8f451ce77d6e96e34afbb9144ae9a3e2cf297b26be3411d48&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

