---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665IUX2K6E%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T030047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECMaCXVzLXdlc3QtMiJGMEQCIFA4PuHoDpYg76SE6qdX8wXmKJEJhrgZWgNwrSAQd%2FlVAiBXNBHq5HbLv7KM6JNHGz6H3rMeebOGJej%2BFsSZpMCk2CqIBAib%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMTtfGFokEju9vrnT5KtwD2EDLy9aMX93HcOGbKju9UphXMsV54xMOLwBQX%2FMZPa6WnFrJac9fvGcljdsK9H08NE%2BHEvJFr%2F2UQZVbFIE6sWm%2Fzyn0m4sDllWl5RFFNOnwJW5E4pPiCT1OKT3UUm%2BXAizwWuzN%2Bwia94adLBpgih4UjHolBE26AMBmKnVyAOXIXJNgh95nziokBh7lPOn%2FypzYLY6Eq0ffC9xgC7UQZjaABtw9cDrqKIkoFvCZrRS%2FjUuZvoPZtmb0A3E6QJjKH0WKICQgMluaSIWeYpsoeDO%2Fbx46Oe%2FXqNfDpSRYWVG2WJHl2ydKK5CC%2BkNkGSdXchjXgChWM%2BNyD66tZJn0P3co5S%2FQDNuaZImFh3uu1%2BS1Rk0sSqTmhI%2BoOPHKNQ1vcgSUAdOz11%2B4K3%2FdRnLuGcM7oA9h1fEOGigm94PpaOavyGu%2BWpVD%2F6Z9G61Hsa1LoI8a%2FWS0xxJULRmPLUp3ln%2BllLOurB99RjewA1WomIrup7RnwMkLUKtAzs3GWZGTL7kfJG79rJ37GOtINeUr1VyaWP6KGl5zCMIcjQ9TdIYLkk3hghfaA2eC%2FFAR2f5zHzW4LP%2BPFSp%2BE9bp1uUbrREe9c%2FFPqMehkFStrjFUFissBVW1YqA91V%2BVmUw2rSoxgY6pgEBcPD2PZ%2BHWosQyDH7qOJNotXDhxiWSM9V%2B4oQm6bwQZw%2FqGGj9spMnc%2FLzzzLekNhRZMAycDImibJNH%2FUvxNrrX9DbCptfOmQVehvalbAEJERoeGZJlGyib7iPsUTz1DpNNm55sjvDTXf7VEa0FD8UKFqFoREJvmquVtGaD5oBPXhkpfKtcbSkw2TGJDOxajy0Tbtcv1u%2FZvvAYBRyMfj4CUD7oJT&X-Amz-Signature=d799fc48492acb91b6904ff72deaac2bfa4c971ebfa6f76004ec23a3eb62e003&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

