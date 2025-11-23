---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666ZGZL7UW%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T050042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJGMEQCIE802NWvexTIBp84KGx6sNhpsHu%2BVT2FwGMOdIy6wAuFAiB0xn1OAmmJCIg6kb7SGtvva9wCtFkFPr8Qdl2FjYR93ir%2FAwg1EAAaDDYzNzQyMzE4MzgwNSIMRZyVUNmpcZz4baWLKtwDVSqh%2BiyPgp4BcDUjZVu%2Ft5oO5WEeeZm8PZyeGiEmsoyHxe8uTNZz6ga%2B4gPE%2Fjp51k4Rfby7HonTTll5Oq%2BXg4qLdX5Gse6dq3fMWqSCrarknTVHYaNOOukYIDog1paPaEQXMMUnmP9nsoVCwldCupDJfhAu1q%2B326Dy01y%2F2HehK0OcSiV4%2FoCr88w0zmS0gHbyxlMoZZxcXPRHEAOnZ7ALQoxW%2B1zo4hqpBxozIpHs3SzjJKuqdC8u5TFMcOMa7eFBpkruo8piFZ3RzuzZyuxNOLhMgx%2F0mf%2BMnq8YGdOaLDa4g7mP5aST6aIqyZr4pRrSzjTBhykuclc%2BVooBQguprCxZ%2BzlCTxG13E852SkecvYPPsYqPCTOxDBVPMUSopccCwt4ekGteuHLFnPJJBqB1523Z1gDLiz0Piz0Va2jvlTClwcIfZpM4TkXsEGbv2d47h6Bl2uxzvOcpkyiEuBtHVpc0N2c%2FoFoJXVA8cyTqJ8FphhCzvbzghmj2T%2Br5P61cxOV5xU1%2BNjVzon9Zad%2FdJRtU8%2Fkxs3uqLtVnAs9cEfslSq4law1EiCNHsRaFkXMV7qr7PRkbuKIYGBOfQJVDhv6SPmmAIyMddTp44XqWLo2gyTHJLy%2BEcwwm4%2BKyQY6pgGCoEbE5A0xozLDJklfQ9KI8sxjQOqm1YUCAwZaAeV3QhEqarrF1mmshADv5FffMc%2Fzq9kDNRbFe78RLUl%2F3lhg%2BoC4ZqaJU%2FOLiSncMSD08b5bquKPB6ZwUp9I3VP3QvYh1SoeMP0TkXeDKgO0Qx9DIRH8TKu5VBfM9Enw%2FY9ezjRgbXb3%2BaPvcc5TtWIjNTyalr0RaicYVjrsQ84hUMVQlP%2F9CfHa&X-Amz-Signature=7016ccaf4edafcecc31c7781bfebf5496ae6b5a826ee13703dde0c2f3f260b2a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

