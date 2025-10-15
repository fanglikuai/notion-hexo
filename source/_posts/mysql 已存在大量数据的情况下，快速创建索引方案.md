---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZCWYNWCQ%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T220038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIB1c%2BF70vQRD%2FUv2zDNTM8%2F3NdmP4ir3IPeA4%2BHsue8tAiEA5FAf0W0%2BtV64Q0ed%2FXiaRONC7Vk%2Bg%2B1WYNXhD9u%2F4sQq%2FwMIfhAAGgw2Mzc0MjMxODM4MDUiDHk312%2BfFJRUkXYlgircA6jAGmEEUhgSKCnkfxGSj2XGx5a0LMRD82qlrNDhh2sh0HJGqDh4rXlvLFkseRCITp3VlBZeor51nyyIChMamYuXq69D8Lb0vYT5qg6SC05R0%2FkpVhBdAi3fiXiks3npz%2B0uFbAlMP246nNEVefn0j%2FbA5th6F1RT09fdo6Xx08s2Igw4WlodjLMA2zCkcvS%2FKMmyiO5Nn4o8J7E2AGGJStAIGWAPBUFZUhafCxSrbJOomiwzo69Jxip7c6ZpbtZ3UQ62%2F5zBvUYd2FrYNloiiLHvAoVxM%2FIS36cRvt9kDoQn4l3ULCnygUPKkMiIK5KXSmHYgjdjoJwl9pG2ZvDGR0SspaHPD1cFxBTEu9Pmosy2HjtdeRS3WM6Ulgo5L16dGAKvczsOKI%2BrdzKVa1bxhezY66tNRiuvqhPxQ7ZIKWof%2BD9CMvcIvYgVNFtnRn4Fl4Gq2GpDu%2BwXOcbP8R16A3eZRTxzPkGuNhG8nJ%2FOkx0Zx6hF0oAzFoAwNQAWc78xPAC3%2FieU7FL2%2BuXg7a7%2BXV1KH32r4NAuy0s87imEbFF98eaOZ4665JDJpy0DLwijZZXVwJiye0h2BdZiVFplN9cdzlvgqfnOGVGyFXAcPFBFEL2NhLQ325hqyQbMICQwMcGOqUBJBrEG32whBV5xPCNjlEaHJHsFLXCRN1SctfmDgJmcSv%2BnT2ZH39xAvgOHH96RChgoOI8pPbLtbDXQKhViDKvegZV9QqtTgHXX%2Frvgje40%2BUdDylhXyLOgXxOtppcsyHTAWgRw0e7DuTjBPts4zG4855ebyuNt09Us7irIxM%2B1OfTcVkryH2XxESPEma2LHHPEyx3ui5v%2F0bESgnQL%2Fxh%2FjFjLUGz&X-Amz-Signature=50f15c5d5962b5d609a37c2b0cde1cb4f2944af8575b10d77bf35ba3a32650a5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

