---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z7VDMU7B%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T030042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDqooXaI8hmtb85cX5I2N%2B0BxaXQNAPitHvnFRgPLDuvAIgHdqk8ZYBV5xgTDiSxQjQlqBSv0mWI3fIc4QwC9tguyoqiAQIiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDORE%2Fz7YyQwTBpm3mSrcA%2FOx3QGfbG%2Fq1dV1Nm8%2FXYEu%2FDVeqbES2aZLKjKTOwS4xyiCH4bku%2FPKtdPThP%2BvHAKNv7b32nrSooh0ERl4ceSHgXksDqjNxTnFAKu8LKSVm8Gy%2FpfBByr8AR%2FrVeRsAMX7uwZhfXWCzj9Wb0NdCD6IqarE1R1kdiMBiq7lNFxsa0TymDjMGfE3MBIu%2FtlQ6y9iBW6LjKDa35vz8VpPRWh6HXRFTnXrGu26Dx2Ty5yRObr2SNI64iAmR1xg5p2YngJGXcpAR%2BAntu4Ewx%2Bkx9TtEQqBo6cpSvQRjtvw6JZPiMn4QmcbjTjKsWwDTreTVZ%2F8%2FP%2BYPLIzb%2Fi9fvPT71DNepsje27n4%2FT7pPyrnFnmmcE54CtjyKIo%2F1tTXebEQuFqx4r7YdrL536CzH%2F8Cf%2BlgmUgA06RTMc98mAjvNl8tsBewBl4X1dNHQXscxFUYtzIp5zJgux10mU41giJn9PoG9xVDKTVja7tKtAtqV88xeoojmFehrsZlOxaOavh0eVc2MXcPsP%2BnLe4ulFfs2J22grQZQBwi0e67I%2Fb5gfnrY%2BWrXhaCbq1i3MM%2BUJn609K%2F7vMLZ8EKI8iXqjCZ2WYuuxBnlIvOmzgqPNamD1ycx%2FeuWd2k8w1hmUqMJrP5MgGOqUBNv5hcho%2BI8G%2BaH3LI8rsvT7r7eJWZZdzS1rGZ9Ml75NwgfymsQ5Rdg7DvjgyOTauBqEWgawd%2BCVdD0ViGLBL7LNWlU91NVVtlCwricb48ee%2FUDvKhKCsQygbYZu7Vdt%2BUacc8p2ymYrBcEzrBlqHDv6pvlMRPPhYLOKEWRHIRk4iRXV5xHvcvhF3tLbdFsa2KrYdc%2Fl8Su4T%2FofZOFZMfvUmdmdA&X-Amz-Signature=cb1623c7daf6e67998ac5ad9cae1a8f0fc1dfe3972377ea39034ec4707dc6209&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

