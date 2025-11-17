---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667M2SVHDE%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T110048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDt8%2B%2FclGcVdX1Njs1ND83bdQRNIERLyztTQUk6rdfsRgIgfx61T33tB8dMyooD3EBz1%2BdExD%2ByecQS1npj8upjb7sqiAQIrP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEQuzUrkd%2FhTpyvMvyrcAyl4FS7lDvd7JoBCUKJgFiAvdbId%2BqQmTQnDoRgvoivjCMi9lD8YSzE5WPM5doLah2um2QQqtAZ%2FgdIPrLzgW3WPi16W7SDTnR6d2Sjoydq5H0qJUQd0lfHkXdvQ%2F0U%2F6xm2Xh3Kxs7%2FPbZLSMgL3CTF1rQq5s39SIbSjex86erlJAW7Yw3K1OaBBOHf8zY3u8J3gc14u2%2BcbP%2FI%2BNY2JGg%2FLWdjQLPXAbHT6w1rcDz8NNEjB%2FB155MtmSdjCv%2FBXqS0XA0sdKtMIgTWIWF%2F4dAz%2FxnooU3b05H6kGUqLo55BzqxXZZ69QlOPq9vxpeLOwkefV5z6SccUCDbhELkNcUFFNIrKenm5AvMCLyFdkIhYKhwtb%2FX9LtVbm0qpaXb%2BtwS5o5h26tTTQcusxAUS3A0lEtnWd%2F%2B3zg7YAretlQ77q5u5qiGmdmFrqLYR02u%2FF743Hs5eDwEd8kH9CPElZVsPoOKlPQDxkPLbA74jKpbBUiKQITZ%2B7lwJIPqN9vGuMgxdQEhmNWBAQPl2soO2y67Z%2FTd6Ed7vER0y%2Fabgp5dybpJDZG%2BAweKfEijcRtCBR1RlJy%2F6J5V2vCTyv8NNsTJBsJCfVsMbHmysdkqk5QJdPmFvyn%2Fs0%2FwXio%2BMIv168gGOqUBy%2BrNiB6TA88WIZnsgKFb7swXX81tf7G%2FSlQfxdbZXhpbAanSXJuzDO85X%2Fy0RuL9fyzo3OuY70lCXRGwOZaRgdZBPCIOoq6PbchcsX9aKO2RHGDxCT4PAzMNLRcH8G9A0f%2Fa0vckVBne%2BYXS2wjHPeW58Xx9UwLisRSfDX5B18xniJwD3Lk7r2nCkLJVFKmvbMudIC%2BWuXAk5sscCVUVwauSdckS&X-Amz-Signature=19d38ff32c70a01a5b96316198ea1d0fccf585009c0344fa33c083dca6eee029&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

