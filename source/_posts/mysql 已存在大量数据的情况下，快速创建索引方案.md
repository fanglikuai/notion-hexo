---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667BHF7DD5%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T230040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDISBXiPGPzMoTyh1fRW7TzVqKN75BJZsV6U5wdh90fNAiAhmtxumBgLzpdW%2FEObuS%2Btm%2BzvOY6SExD4tYiwGUWPOSr%2FAwhnEAAaDDYzNzQyMzE4MzgwNSIMkVSRsv%2BiIvdah6TNKtwD2ixj9tPk%2BYYf2dwa%2F9vDHAXhE%2BSx1RRXLralb5mvr%2FdsRDkXTywd21h4zh4z5CLyCcujs0fg9dBS3QLoejMmQCaIlhoZ9wzwi%2F6poeZpTQs%2FQ5rzMG%2FZHKCaYrIt%2ByWbu1gPoAQff6KP5Jmrv%2BIIeQDJMxQJBpTotyrfNsUZmCQjvPMlZxM%2BE1wkimuu%2B9smgOE4SfBoO7rhpPfe50Gtm8mp%2FR5%2BbVmJByGvetNgQHFRQ9f2KhMRu%2F0ZWkndaChTGD7jHLI%2Bfgy%2F9B7ZcLC2a8aVt%2F%2BtL8Sw0ymjsPj3uhbZZnB7%2Bb4%2BBWMUcql%2FU4QKAFLL0aWEBWPCC2wsZ10wQeYt7VUevSGYPqHh31CQgkBO7y8talfdVA9K%2BaF1i1mpsUiNyuC%2BPpt519Eny6uE7uR0jDm0X3nVFbaQF%2FEuDfdODib6%2FAv23TRhvA2M03PTdeuo4eUNlA8vO1Ozuk9iO2qJ9xE4M%2F6HLNpDG7gbafDLEjMIB4Px77c1kSfdtBTODvVHzYFJSmsposOdWK4m2%2FRAqsb%2BRpAprZor4rwLULQriHXFGoogOOq4VZmFTZuhJeOLrI9Rec0cq4xaraZdYZTaTHLvz0Us4yInyFL5iOO3i8KlZPhkM1UAmhEwvrGGxwY6pgFRCFuMYILBlVXehvgaTViJo%2FGdnnC9Qw%2FGMpQ0bUJi27QWjTZHm3ZhqXXV502CMgeS6PaXIk4IrPCO2OXzIck4IWyzChXZr67BvA8aaC6bf48GenLe54UdXJiohJnoepb43Ari368hD2eBjz4pnJlfo2%2FHmnr7W2Kz%2BxwacX8ykFMgVFATFXZr18YyARjhzUylc8Wduq8ZWqzR%2B%2Fm4W8%2BgzihffZEZ&X-Amz-Signature=e79f8dc8da82a73aaa2d20b139d05363a87e2805fbe7a3f3007abfc56dd98655&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

