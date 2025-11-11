---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663ENWCXTK%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T050043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE0aCXVzLXdlc3QtMiJGMEQCIDQQ3MrjUpvF0I0YW5zI6kwT7jGUe6AG7t1v%2FVMDUyNjAiBBkvGBBiUXmNbrLQCvZwmmU6HB%2Fkeo8%2BxJzLnDDdl5fir%2FAwgWEAAaDDYzNzQyMzE4MzgwNSIMkXOaTkB7yrJw0ZPJKtwDKHD1Z3eCuzSkPSCSY%2F7Oa%2FnLQuSV03vv%2Fzyf%2BE947FEo72eL%2F%2BKTsL5Uhe1eSsDU1W8SPMoXm6NjzqhIkeOwV2OcHuQWhuaVLf8bEIqd%2FRaeTjmA%2FksZu%2B68tOCYMel5x4FshxtWR%2F%2FzXf%2FMeUXktKOGu6YF3SlU7rS%2BAoMzo8kV2glmKLRG2yn8fCOHtQtdGD1mf839xpJXR%2FDC5dz4UNw%2Bwk1tXKNaVHsjXcTr98aydT1XjQkV29B710MUseHdQSC3ps25MvphGr%2FXEye4gdTEqg3Gz76%2Bv5w493CY3LXVbN%2F%2BBF3O4TC%2FVi%2FxW3kYQAev2ONrWpxS8iUdA9mrwHICCzj3xsHMOIzYueoI0KuyM0sfXRiK%2FXmTzzUqydMfJcotHNIZzwO5T7H0Yz8D4tW1OLrABVw73N%2Bx39LPQo6ntS9zKsJS%2Bdxoj7Qod4dAvT19jydTWyTnhyZtSgEHm%2BaRdGSB4KRis9O0h9h48jrPOh%2F2mqcvUaA7T4PWYQTRQuk7AH%2BNFBX%2BqnOaToUvCzJ36kL9vqZ82NBwDy7bLIHPgA49YdGHf24PNjXBBDTFSAR7Qn1f7vRQpdr34JQcqa8foNd%2BXhF4FQ0OeJU7w9cis4eK6zmZd2eydv4wr4TLyAY6pgHViQhpA%2Beoq0FYgCoyI04SkHQVRmHQ%2BE4qrM5dsFgLLx3gG1MKS6w4GVyhSeVT5z6HApGC%2F441YZWv2%2BkrY%2B%2Bdq4VA392iHgYeCYk67qBf2DHP1z62BAk5kbAfb9%2BXlK839SGT1Tb0M%2FJeAj%2BKBNVIP7bNgV9NwZV8gj%2BdHEZ%2BFADjLlIdK%2Fet9%2BXxQ%2FU4H2nyDSUhDGq7EyJXX9NgeOcqmGpHoRX5&X-Amz-Signature=ca4f74c697546059d46c3611fdd1626496f958bb9cf39be5f14208697174f8f8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

