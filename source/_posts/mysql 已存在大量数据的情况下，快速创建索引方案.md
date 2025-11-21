---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663CQ3B4NV%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T180053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEoaCXVzLXdlc3QtMiJHMEUCIQDP52q9PNTe8zWZ6dpYR45PtV8vTX%2BxEDhGsbHizYgpjAIgGqvlYnkH18hPXaOpjkt%2FCvjJ0kaOG4%2FUi%2BtIPJPSnHEq%2FwMIExAAGgw2Mzc0MjMxODM4MDUiDLutctyKGLIP0xFxFCrcA%2B2SWAIqoZE43qq0hh9bKKMSTf%2FgWElERQ8PBsvGiWm389ztF%2BxcOIJM6xFw%2B5Uhgax9wOqtvgxTu6kerq59uzNNwN0NtGBguojEABx3KeHbocAkI5EkZshfPIXrhG%2BQKK5URFJNlbuU9PytANlHbysUDwztaaTKeVqccJ7WJ8WxQ0IGPtxjlAK9bZPG%2Fa7xMk8VFMdMHxwrOORQtIVCPwI0keXrJtIo6D5X6VsHI5ts23tY%2FySQP7SwVywZC85Vqql%2BCfC1jU7JpoTaROHFfRicEsg%2BiEeSBORo4blCUPTl5XQRHftTSly7fmQfVDN8Q61zWzh0lLNM5RH6O6aziaM%2BaVoZTCbe4GPTAJdWIlQ51A6If0gAD4zMnZarpkQddrAT5%2B80%2BvTDdnrKYNHmzbgnWKvz1Qo2KDyDuST2VlRLbVVKJXt9eP%2FdzZejSOxMPvuW2mHj7kOqZKUbXmAE%2F%2BIorH8c3L6C428lohG4edQnWk%2FnCDu40aGYAfzfFjvXgV2mJEdIJmJFUvzFEk%2FsWYDT%2F8%2B48gBFu2LpRgEkixOxYR4D6LvVXMstgMgbK4rgS5lgIUXRBI2EbcJE2hSxAYVz7vbtATsQa4%2F2IDlOCtP3a3JCR5dtZcNqozs1MIvJgskGOqUBSj%2BqVURJyVXjLhV2RFOf%2F131gcvqQnkQCSr1UVwjW%2BViftDiJSpD7uy%2ByN9BAOMgQ4JcErKe1ZBfKtu8yOzEqli6Ni6kRoQkPxBikgyfVwb%2B4hbK9dbfIi2qbrJJRjUmziVdTq6%2FOOJ%2FctdOlwGAV6uPjgcosXWxaQsMxNVnq%2BUMvQ8fDWw%2BqgDNdHVniu0dy2MxIIY4aG7k42hHswtDNSozdYkh&X-Amz-Signature=7adf780a5971fc5a7d7bc7690bf3bb14a77b0e2bac95ee493d53f1de0952b698&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

