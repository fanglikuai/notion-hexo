---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WMHKOTDD%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T150050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH8aCXVzLXdlc3QtMiJGMEQCIDDWDHpLZD06skkISv6k%2BlWq3vzKZ6V3xCzjdR%2BmXmC0AiANb3%2Bdgz1mjVy3Gj9lxg72UruoDjFqmnB0ECWE34Edpyr%2FAwgYEAAaDDYzNzQyMzE4MzgwNSIM3lrks6wzOAjhNG7KKtwDkM%2F24IVkGtSWztg1BGv%2FlQawgWTTGZw3%2BhhotrXLkF%2B%2FsK1FeaG6hcFmQNNpyE%2BXRm16P2t04t6Gk9TLmprlrINPHcYjFwv5akZgO8MEIBZKNoANM3PpSbFvY5soAEoSXlCkuQeHLc3XIO9HPeBpe5wGiS%2Flkn39m21XcQ6gnMw1zpYanB3g2h7Ad6YDyRPLKUg5Tvqoor6nSmTUfDWkKc3T3QwqL9Mx3TrOShseBf%2B%2BLapGbYaepN7VjehGcPlihLhB0fshmz%2FifOPsLF6LaXKEf%2FObjvdLMtsXbc5Cst%2FcbhrYmP0eDkzbUYJtvzrryTjElo4yF7LBCQBgsDlru7bqaQVbzmuZH1iDU7J6l0%2FNKOqsxGSlk6vt5Sx0vzpw%2B0d9YDUBMtrDecyA2KZk8GOsU5hEg8t2J3PpIM2z2tlLUlyqUt56m%2BFXe41nfLoxv02Y%2F1j1kfu2aGqdWWSDTbrymCR1opd5n%2B6Qwty9rPiWTb6cEVUlY6Q%2B0OYsjJp9jBXkw9ZKBaXmxU2S9wVmjwSVRst9tbtT3J4htQPx34Z7ESCwcxxvBhhzmIH5%2FLyC%2FVVTMsPpeo2%2F7%2Fx9l6QiU2xiSWKAyq3IjMIo8aVNngJEPGbYeburMFZI85sw9%2F%2F0xgY6pgEnjxiVyukB4Lsuc12tZRNpIIC9ZASUe4CaNid%2BclOkBoqfLM%2F4CoEs3whGd3%2BqBR%2Ftx635AdVpmH2kxoQ6QXR2%2F53gsbK98FybrOgHeD%2BRB7IWhVrwGyABZ1KoF4%2BbNjwK5O%2FZw5bdzyZ2HybXnLB6M6EqVYrF62M5LiebquHfsD9YGjFGaib69Ul2BMZSSWPm9G16W%2BuNhCsxn57KBThE4R%2B9c%2Fk1&X-Amz-Signature=46d0334d5b406583fc7a60974094c3a64c39f493d627fdd0bc901a55db0b8be7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

