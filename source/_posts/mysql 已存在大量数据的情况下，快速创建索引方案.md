---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YSHWBTIY%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T210044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB0aCXVzLXdlc3QtMiJHMEUCIQC0O62ONbckHLjOAsv%2F2ubXCg0J6szJhYgR03Jao3m2KwIgRRIVAVKp8LJv2G71voVvixxp7Y4L6lvhfaCRtHpv6jsqiAQI5v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEwDjMSgFwIfdwOz5SrcAxQERd6kmmif6OTdMCLW5kU0coHV8R6NWDB2EMtTk9CzjHHfVUAsE3gcDPvufLmuzitZLKVui%2FKMnAdIVAJg0LzAPaxDX%2FCnJLahA74L4Efxhhjsji4yizfzuPD9lXiAVFLFQjh3onm%2BZAw75tYDyRHwGSFrQ%2B7ZRZCUkYGP87LEyozMZqCLxTnGyLOPgS0P%2FxRxi6Bme%2Fx8yZka7VakrWLRXBH4ESw38GPtS15BUGeoblljRYsAKTuNYeybPqH4gWC2wXso79TOzGMpLFWlOGL%2F2ikXXpsbEACdKzh5ebiXgCfL%2BfFu%2BqgTlHsjpD2TRbTpsbHo5joSkBoR%2FWsuepS0RrFGwFHskD8gVNMYUaRds2nP2Tbr0txlx8nlMj0hvBXOccGzGlaIaB1CcVe3aLVnDGp1Ct0r5s0lThlJMjuJOGgrmhRcJWOSDZw8%2FVtJdWDm5mwnRS3lkmmf7eNAaUH1C73DgdoT5qw4pM%2BpPfiWvkR9Hercl9mKniMueBM0iYSHDaB3E4vb8idjhR8lzsNKUGHZ1I2kHScZvyrIcnyiU3brbkt%2FdBM6bJ%2FHLSEvQbbci8KbSSehTjdnRyNdjE5D1csDERSWzSyPiV0jLUA1YTXsXxjDVHEN9JsvMIva%2BMgGOqUBi4To9y3%2FjV%2B305F7Hv9QZTHEK%2BoXmTjsyqtntqtYn1R8B%2BYzc9kjRNRL6enympGGDL5%2BmDwI9JwX6Rh32PZqDCxvq7CFXxrmd63FzfC4e%2BFVa0khExlb%2B%2BVkExYes%2BqOr1eTC2CsgXHWPloUbHnsm2u377M5O3jdaygYOgfEy2sdgh6scCc3ZB0bCI%2Fem6Ma6z1YAqXxl73KeEqMpyIG%2BZLtlw24&X-Amz-Signature=251cdec38881106f2a7e4cee63a124fd1e03a5ed3408fea59625b9a4b0a4050a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

