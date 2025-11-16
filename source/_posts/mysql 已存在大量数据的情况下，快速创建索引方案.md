---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XIBUMW7O%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T140048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDdteF9vasAY%2BO4KDloBN2tYTk2SQsNTgqQaaIamz0LmwIgaVu1XXorcynFVgC7ubrhCFW0gYEw5rRdwsVmi0zQymoqiAQIlv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDH9AYBnOU2y8GuONISrcA%2BIauJzEIOblYSZrms13ao0JdwVX3HM9%2FvcpG53JGO1BhNZ5zzT3NYSnk1SSZzMi5PorHMDy%2BD%2FWMbKIZMBiKx5OWu2Ki87PKt78pbABJWysODQ5ett2xF5WpLkNFifPyiCbgZZlJErcre9W7BwAonBIT78qnSYjp5CkOLm0KFhFkMc6g9DEA%2B%2BOg9Jaa%2FRsHKqVYlMiI3czgTHl2hmRtFauK9msKcwazoEnLy2DWeXo6c5oB2ZQc6YF4hKBlCeiLPte4jQAguDyQAHjC1H5UfdJCSyMCxVssefiUWIWpBc9B%2BaUg5URHnv4vFEzAHFs5DoLhSTuyMQ1IU8JSosE7Urn8c%2B%2F55GngpzG0NZpLAN0jzU%2FyBKvV6aVaDuPkB4u03GPPZi%2F71dJ%2FgD9T5Rh%2FBzHDPs782dFOFuXlBKWOH%2FGJyg18iCle3Px75fOd8WJaPeoUjp9vMExuuZrQLLLsiBmU%2BlgZ3R38nCI%2B%2FDda2TaCJPrXjHIfIfQss1ZSIzWomfaSsC1N4bYQAQOBSE7FnqtkcMffpAOPf3K%2FUCMAnm9f4NESFvP4WOHfHV14NybaPWpV13ZnjUxYgajQWzn3%2B3x%2Fr5%2BnpoHRQ6MPqyAHKW5iswNmkAdBxjA2bhxMKKa58gGOqUBK4exemkSaHWMfYyr1nPtQ24RgnnWWA5OfYe6EtQrbI%2B9aEoyQnRyM%2BLTxHz%2BImeos4zToJ2S0JKzuE7TtkMruhPddqr0TxWOUgdau5igTCFXN6KxkDeflPEyFmb8y9jfxB3xqT%2BNv8niCRqVjyxIVDsSrR3b8MF5%2B0QajFEtXJaDuuonLFBz95KgEG%2BtQEPjWGca1XsvqyBaHPuzX4a9jpuqWiN9&X-Amz-Signature=10d56630553a327573c8a814e6536fdb0cd78dca5f29cfa163638c93ac8a0b5d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

