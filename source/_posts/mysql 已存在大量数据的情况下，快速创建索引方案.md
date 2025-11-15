---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XLX2MVEX%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T060048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDyJfcGqJKZGfx9pdORbmDKjps3Crsb36FDwIZRqFgnaAIgOsLGPPknhkKIfQXuSTyMpZvr5LgcXMm4DVT2uqCrxl8q%2FwMIdxAAGgw2Mzc0MjMxODM4MDUiDJQQNw6yxa3EARKAISrcA70XMfmOXoOdeH1D54RxPOmeRwCHD6VE7WojGHcGHfetkwhAOzlvgM3c2SWaYTx9w5VT7jrb4llpHmFyNK5nOGCjAECFdhWvXQBYRWJlbJBxJfoZQOSoMTO8bRB62WbseVKoy3CA5ZeM6NfOlQD0lm%2B%2FV%2B9LL7U7uUufJ%2FOgSKl%2FOc5EgRIQ1WP5Yxj9tIyDATs44gC6c0Bt9V%2Fm1YgLTwZ9fKdFQXPatjoj5cr%2FQmEIGIkcBUBhloGRM%2BOBj6QPwNd3KwlrO0DRLrC79M78QH13K6p9mOfQL9bGOkCj9TlHN9qWTZ8eVDLyiiuSKxoHRqdIauRQA6a7DWMtTYMcddS2dxIJCqJaBK76VulRPwsYtcMpfQqOYMA1baDcKCD4zScs78y7Y1i9X50g5QS0pNmr3%2F2UmcabTEUbuCkHaeF1gBxPkbxYCUknwgYC%2Ftjwzu5fCZ94pWU6XSy7HMwhbBf4wDRt2250g5ZQgfrZQaODhvOtcXg83SSrEsk%2Bbh9YcBuJPDWUj%2BC0%2BXSSrR%2F6aEDEZnrtJdsLNDgmRF6HGsKxGsXmmmF08kZ5qWWCGfyz2Tmu8WcfbbEp2TPBO69jqV2Zaum801VfKi0drpSwAFWVPkGsUKt6f9iFZ1WgMJOi4MgGOqUBAuCEj%2BonPcq72o5e4RfwQYxvgVhT%2BrXghJcD06kqk4qwrloSDTgO1qBvEZxVpvnqyDdYNy2%2BX0n4Yw8sIj2a22rS3A1YUVjk62gMsdrMOwTqy2VOrhAJXhz%2B%2FQiWHuhd991nkEmsaqUZdV3ZloQDVMutfmCBlCG57YV7336Yo0ab%2BlJcEQJqgv3p6GYFPxLolJd0CF3ZBZnbGZkGSoVhGo1csiQY&X-Amz-Signature=2e6f1901a77a2b6638ee619b2f840c57cc782e800602ecf930b90c8cf6c10e0b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

