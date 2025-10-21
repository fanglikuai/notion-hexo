---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UHAY5CDD%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T030044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFMaCXVzLXdlc3QtMiJHMEUCIDMZYCBmk%2BSG9TnoKCDYQzaSdQkD1iJqIJRNnPsxBLjzAiEAktKzn3ZsGIx50c5Vdtg540%2FZ8%2FJhLhf5JkMw9SzXlA8qiAQI%2FP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHU7O9H9HNk3y14CxCrcA3o8jxr%2BqvTv6%2FzKr4nso8dVjRYVjqGKVenjVN39cLFdxoae4%2BHFljWSv9JWYRxp2AAz%2FrTMkCdwRxtscMleEWpGjJIJlSmogeHan8aNsqDHjqx6A8mvAd%2BYh72unhGyQcw9yYLQ5H%2FEazcDp5QwsMPRQRSQt0NblJbvC9EkphLsi0nahCbdE7x3ZIrib6IJn6dwb92WLhaIsSH23MY3U7IryAyavbCjP%2BJYL3n7z%2BLq3eDFcHf5Ij8P5aRgsLYKWuD6kB%2FhmLgpvgkjAW%2F%2BuBIK1i2dJJE7mFA0aNvGat7ODkpJAA68xE30mcaf9Iau7DcVP7lthKPrMtQrileGfUXB7Ym%2FWc29yf8u%2BAKYBoRdf8zCrq4GRxDt%2BT4vzE%2FIxI9k4ug494gVqaZS%2B2kRVPCRsppgS9kpQtYQEtFhG4%2BDeJQfJ15nMoxMB%2BFWYLdAkzaGwAOs5peDtwDHhOIxZ70xviUvtic%2Fps5InoyYxgSShp5lRahIaiIIz5%2BKiymbClQz6uAlw1VwmNikOyfqbeQX%2FYt3LvkVdtzpdN07n807m2OSQvOyTEiPtGi9lbBsgEnC61DEfoCAkIPlOqMdSaBSij1Hxs%2FAHDX%2FeLeIKB2dGhGrmLimVfUVtrtwMLjq28cGOqUBzlfHMEnKqGzxlNxKDeKrTFTCTvaRLLSJxrpckTBDDcQiCFasP72oY%2F7K16rzoOH%2BtywE%2BeK3cN6Jd4n1%2BKaRRu0Mjao6pmGerWsGRIfOH2%2BwthUuRae9aGvRwGlqAkvq3DgmSc10TJCB5HX0YlQC2sbDZEh7JZkZuKDFHXZtmUW0FPgYkfN%2BY8YG8D2W2CkdLJQnOJ5BrUXUDrDTcOyMQ6TWv1HS&X-Amz-Signature=110cfc746c7975f197cba32eb05cb977664ffa28d724800f95d7788dc4d20720&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

