---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466273ZX6ZX%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T010050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBroSX15VoP2IQLTOL6PzziDO3HQnEwSlbE2lfkdjVpJAiEApUeFZR%2BVuFuSuObxkArreILjWVQNQo4R%2FenH9jSLTWcq%2FwMIahAAGgw2Mzc0MjMxODM4MDUiDE0lTOZ5g5ho5Fwd9yrcA%2FnykHaB6u%2F3%2B%2Bcs3Fyt8yeazukZT8oIU2dmrxJ9Yf3ByuTMu8YGmZV%2BxJgcWIOKFtZ07eV%2BOhspSLdLQ4Bzs8AG7K4VT4kxBYgasF%2Bf%2FLLvKmuoLj2QiNWJBBcoRBIzfr3q4sGQDa3ZeMrNg3iIdlPZuOOlEw7XM%2FRR8YKqcjVJKtkAxBhB46i3QVL8DcLGbIbbMZNmOu6dYgh72MpOl8OY44WCfgmRIVmO1vm%2FgSlNfgthDuJbFWNGU%2B95xg1C5YxxkEEGfN2wbq0RdLiiN6oJZXthemInOu8c1XNbLmXOiilvp9KtnyW12RuU6kIL24PeYEgsiqs0vdnOSEYdtRNXJ%2FRXMRI9%2BF7StPCt4gwVpVZBNPxTcUfckgc1PZrrM6j9dFXS%2BKrQqSPltf8s0%2BFIrUOSdwOg1ncmt%2B5oc9mkI%2Bj577k7%2FTrqP%2BqGF7ENrnRsdTWb0QSedyPxan6TgC5C4vivgEQCTHUSAn0ckfTn5slYtij%2BWG93%2Fpcq1kjxobM%2B08foCoEPSM6zPeyua4Evo3NxFrgMKz96ehRzfcyBx%2FdSGBj%2FQd%2BU30wVzKBcahFLYS6ScDUSxANJahWXmKGUmbt11kfWEp4Vmtg%2B99WaV%2BmXDIA1Q%2FwGvPEdMLmz8McGOqUBu6KCaAjAR%2FCP7ASnU8A0yHI6K2A18YoX5gFZ5Og06KuguSdSu1LmmxKsZSj6oikyBhEqEFfVYIQg0eDqegPTE5q%2BG%2Bbb2YqTWrL7OtuelG4%2BwFrgudWzmsZvnAHI8q7PRpDgRondNlAHpnut766t0TQoC%2FoYulttPaFpH0ut7h%2FXhYZSQSyX507KWtv%2BBESBo9Nnj33DIvPLTOsfnfQsY2gkcKG2&X-Amz-Signature=f38c5d2836129ae7d6962e5502fe0a6bf24fb6855473053ebe16e1e212390480&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

