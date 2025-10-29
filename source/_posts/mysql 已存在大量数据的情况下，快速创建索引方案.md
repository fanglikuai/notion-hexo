---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZIAZHTRT%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T050046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBMaCXVzLXdlc3QtMiJHMEUCIFkLKS41DDR1ism0bM6WSWeb19d4yB5MOxnePYNBlIpaAiEApHLnsgFNArbTY0Ha02Tg%2Bzllxs3KsuS5iuMxmom0nUkqiAQIzP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFEsv4ALLCZ26SDnWyrcA4eajXcLSq3PkpVYiB2L1i4t%2FeqronX5t5Cc5bWaWK59QbuDCLutxyGqMN%2B%2FdNsFrMmb5gbsEoIlecOj0jEVBeW%2FWHQfcQbwD79NNLW8s1F3N7vIimHJJIfPBPF0jXXJYvD4W4CHE%2BRs4KjcbaioiLcAqC7LP%2BAJIIjpKvnkG4%2Bj79qEt1avxv2EvvyZ6%2F1pOwp3Ix%2BZbBIHQf6zSYHho%2BnscJp6rsTVzYiqbIUb4ZrDz7jLbd%2FmIxB%2BJwN3h1MAuHNDaB5LZD8V%2Fbogt7fJAENlLxLB6LQ2GO45ThtytdyFog1%2Fa%2FJKr%2FFZGQTmvRDYCOMUr%2Fqu5P942WZ61Q266F%2FRSeJcgK25YHuh%2FeO3wIqGhlulDcl6jeZEg1Ic69tOmMb943R5o3FnK1YP1521qPyA9f4iEYme51%2B74wyZolA87%2BiwmDkxUKyAeclrE7AehBX%2FpXCTf3oDe13oUusFZ%2BJ67uR696KfB9ES47H7U1vABCYKBjns5qv07E62BmzAVO9tBaycVfwd1gZlFVWX2mqOH3%2FWfjRj5fR7N1HUhACiyvYDUP8qlWdzUfUssc7JU2PXYBbz6%2FBkO1ChcFv%2BtNxRhe%2Fs0vHx%2BoBYsFQodb5rt94j%2FWFeQBNtDA5lMNWEhsgGOqUBymVdrFl4NNdHbaWhti7kF3279Bfq4yKr1Dk%2BFOTIfPZWj2siRLRfhpUnQ%2B33PbB3pwxdRsIZXXa%2BOdotgxHlbosMrt0Bdoj%2FO7KmTn1P50l8zcY3X1VI%2BJ4anQcIie%2FvoYxFksJEB7gdRvWg8H%2BpHUnIA1ip%2BR8%2B4DhTen4kbaScqgjxu6JYAGpus3BCg8cYDBAnJ6gmG1tbKVqAiInEHP%2Fmzfph&X-Amz-Signature=4155e02fc874d286b3b923e1c01a2f80ec48989ef7d7ab7fab162557f90ccf33&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

