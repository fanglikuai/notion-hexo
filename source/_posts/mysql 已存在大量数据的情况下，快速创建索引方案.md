---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VIUVDUI6%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T230044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGUaCXVzLXdlc3QtMiJHMEUCIQC%2Fal4tkOaOzUllmjPH1mG9ASzb5Upow7lL4gaD7xOwDAIgLNrgnLD72Fs7FJzDzbvoNhaAz0Tv%2BqcFJFaVsqgkbL4q%2FwMILhAAGgw2Mzc0MjMxODM4MDUiDN5GyRqMAu0fhxX3AircAzsSl4bsxrfbXyuGBdhqrHVIwQb%2Bn%2BkF8mQGP%2FK%2Fd4Ne5TQ97hM3bJNFyh0GqmtH57ObTq4fPvogJ6JEgY8DXkTkltV3%2BDScHNJoJRkgGoUSMTGGDc%2F1WUH09fZnnh7YkwWQ0Qj199L8F1669zUnwWd3YhDqknDLLy8tjKg3fjevw%2BXw7N%2FflsGmiz7bt1dgV99UtCA29GfNRdVosPYsCbcDFzvDAiZFHbTH33rwne%2FqhqpVymZ%2FjPToPReps%2FTDp%2B1JP6Qfngc97Dlz33ypY%2B4dzbDaicjQbcsDlJNrAFhGbk6vQHdd6TrDyLStR%2F4ehCXJVvLGCVVUGF%2FAQVwcKB75HITn%2FfmaVPf%2Fd6mrZ%2FfAoqSzAx%2Bz%2BSVLLPJx0GXgWpAiGsU%2FZW%2Bni1V6H5ezTieS5yhucSkY%2F5BRP4MsMbl4qdth%2F6bGDnQ0SboHeLMsUtvGG9L4rtMNklHDMBZQJqnOnAdv0vjbfiCx%2FyQg%2FJzUNGOepqYYbVqv1SKVgyB4FrObIE4PRUNAEUrxmPukP17APn5mytozdBGWqE9MyJzwWhcRBwptTWBy3RZvX6GvgvMXiAVYtEP0EU8nO9mM0ihnjhvqkrwnQCBWlQFspnus20H%2FmTf4Cwo0eLipML%2FFiMkGOqUBXAmr3CZN3oHn6%2B8EmMnHbpt4BXlbQt%2BpkC9bV5f3IcEQMMScgBBCSIltdtlYPd26NNmxN49gi5TCLmfUUoqvpmtHqLVq9Ss9M6dhZKVZwouAxyBRoEGaDkE3r6PIkUBUjje2AxyT46%2BdsfBOdSBEE1Rt05TOGFbmkQx%2B0XqXsh16px1hDK46BWTss6B%2FYred05S2hGp0NzbAy0Cup0Qx773CjSEU&X-Amz-Signature=6d8a58438daf2e84641dfd62db368e3708cd8d3d1a30bbc5532b079f153e9bca&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

