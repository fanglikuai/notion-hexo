---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SR6GZUZO%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T140120Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGUHPq1KuSF0qCmRNrFTSiezhK2D458NfeBORHr1bLRVAiEA8jXFxmuIlh%2BqCzM3piLCBpJtjkzNcUxMhX68Rn6Pjg8qiAQIpv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDC6HUabCEcuey4sSDSrcAzJrZt0AN2cESrqAHhJzK3x4UMOqumv8iTcS6T0kdK6XtX95msrJvsThiwhLVW%2FR5e49PBp6okx7IIkKDBDIBKPtJg2ttWfOmvgxQ4pdyYd0Clr48gfng1gZxxRqPEbEapnkpiml1X3ZnWtHdJ4sJgoTGZxTrK4ZK8zP8o%2FnLWizMWc1Vs79IBanu5zZTYXWcxDE1xd721BL2Une%2Bi1fTivEBTWecQ9CSXvw1qwl2WmW5eraveyg00iFnn77uIPmXKakTaXwnrImyS3ZHQbxF5EwKL9IqZZ62rf3cSXt1vzMhcAVhYBNULVQfAylCJDgZx7wccb%2F7E4%2BoQbI2%2FnTU4tIbP1w%2FSX6h8MvZ5mv2YU6LS27gs7cTaOiwrSBjieIWt1xh9ga2OXqACr4y%2Fn3vh7ez9uqW8NBjR5VXa4K%2BVP5MkAf88PU%2BSrtSfj0leTCUxW5l3WC%2F3Ti%2Fpp6%2BO5v9JLcmlTY4hpEYEpjndxEnEChnr%2B%2FO4jEsvw5%2Ftd8w3WHV1GwR%2FiMO3CnbxTV57%2BZu0IrpxoXAjIdRd61YHelGtCaiPccRWVKwmq7P6sBRfW45GXBV6iDi7qHCIx2y%2FULhkMgpuAWrR62rztwSNQl13kixDVYq0w3h%2B%2Bf4FqdMLPc%2FccGOqUB9Suyjz0MZjoy6%2BXEXyFyyH6DkRs%2FkvwfJutvJaf%2BtkdHaigO48Pa%2F3uTb57fLMXJvSihs9bYZBEpcTUMwKznlNj6eTR0ca1sB57aX6Ba%2FYnHO0wUD%2Ftj4oo0McEsNponnc%2FsH85kZ4KCtbjI2ybuYC7iS%2BJKL3kMwXsh2CTtJ7RCJ7McH2S6tEujJWHB018s1zdt5YKk2nKvpN5uZUzXjgMjyRF3&X-Amz-Signature=75c5e824fe8b81e2363ad603c4cf6900c3583d4218f5988489730bbb37887fb9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

