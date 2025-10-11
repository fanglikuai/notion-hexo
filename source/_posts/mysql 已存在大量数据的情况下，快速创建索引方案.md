---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T3P3N2YX%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T180038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJGMEQCIDFBxBnRBlIb%2Fa0dAjevNHImZQzGpzn7aF1385Sl8khlAiBNdFPQ25XIwtGi28YMUeAjEc6z3%2BdUY0xSznr722E6zSr%2FAwgWEAAaDDYzNzQyMzE4MzgwNSIM9gAn79qFL0RCcYvaKtwDuCoPM3uCD6fxQ1qM7oUOQCIdMfqNqzWKejvw6g%2B2gUd5k3t3SvMbZ9P4jr12rrMUQx%2Fp%2FVr8MLV%2FT7LEp35Fngs58Q7djSkpwcFbgvwFimzBpngvxlvKUFehNiA19q1VTstEGgdgyxLYh8GuiH2JwHXLiCMytKFiphlWnz1IOMAtXwknl%2FvCUzHokXRAXCXFBF%2B0IXmkB0yDqnVfOtVhgkNKMPQXExbREjlo5jnd6V4qxprzD0sqOjfF8vYO5uM%2Fbi2liAzP6o8JrkZ48m5lFcFj57uPalwyzSNWvxz2HtDAuMAadGT%2BZrWaebzzy3Y5XUOnDPCkvG%2FprjXdbi4fOJefGbx8Tiz3exsnJIPC%2FPhCqdTWSUvYk5DtYhKSKcsIvvmIK2gNTRtQk9p19raLCdn%2BnDkRLhr0PNalOoZkF4vgo7P9jmbn0xtAlGvf7D0%2B7oGg7jlZQYQeR2F%2FlDfi29YI7kZuoqa11UL4oyNClyBO%2B7wtGIkcDO7Ct7%2B3UBs8AbA65YlV%2BfhCZiLbOFyp%2Bfxmds8pL3I3YDQGIk8lFQkmXgq5VAlgBdstGydbJZCyrn3hQ%2BxZIQ23HdcP3xRHp2tvfvXZ7BejgJH%2FgVnKy9l6owODKczDU8XRedsw%2F6SpxwY6pgHq5mIes30FAuVkrQFnknBo8azKan4c85JwHsguCMeEQ2vOhrIrxp1Kju0%2FxhVlqat9OT9nYXX%2Fj60uTIaUwhll3bow1YiKmm%2F9gWa3apSyO2s02W0MZ6lR8soi3kSJddZkxBPo3KeorHVXJq4Zx1%2B1u04R22KqqCeLB9e23PU%2FFM18ZIJNc5mtfBgfB4DEBxI0TO740Lh4ZcN%2BfI4CKq10dNboO87T&X-Amz-Signature=3f51e16cad938e4ece493484b856e36a5c4e2576adff44b4a84c2ee229595342&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

