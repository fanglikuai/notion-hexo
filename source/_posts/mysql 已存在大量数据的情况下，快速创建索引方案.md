---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665SJL4URA%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T010039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJHMEUCIQCmSwusZmUbuFTmMwF1qwchWCNTe5Wtg0ZSUH0pKNMuTAIgYRY7QJBL26G%2FGyfmR0Vhvx7DIS4tOQqEpVAZCwYx4XgqiAQIyP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOxBqZYd%2BzeeM2iJcircA51b0vYWWhaYkD7PoAP%2BaeRa2J2h4JeTLl%2BKhTHVvS2afXlLdJcTNi01sdYxvnGPfMR03kN9D24t4IIVv2Le%2F4zROpcmxDlvKhBhnuu3ze5hHseEZZFp2F8SnnjnMDdIcMZqyghSWuDY%2FAXJX11BZOQxwJ7Q1AS2nKdcs%2BTO%2F9VekSWefseftUWe7NCJ2JeT7HiUxx96qxVqux2UJQVt3fciKwkOEWaaFYhaO7gz6GhY8Jqq2dQAsd%2FYfgTJTin8DiZc%2BIMFEBJ40XfvJVIkC%2BHyV%2BVBNW1DkoPlFXfEMSeJFtaU4cShVcqMgwCMFxWrGbd5RYM7J1Tod5t5N0Eh36QXG5hweNJi2san2YA7MBC7%2Fs7E5rKVm39Cibly1ca3tsLZXLTH%2FNpeNK5UEqLT%2BkvuUed%2BPhEmTsNGr%2F1iJt2AQEtuHJmMAs5E0qHsLRmbJxnDQStTVJV2UQcVLxsrquDxh%2FAH8VtM1%2BBBGqs86hR76KncTohL7s3SwrEMs0CDkOKQl1%2BudEiYh%2FhwbhAYUgZ6eo1Q17nd3JJDClitoRwwerd7SK%2FSCIOSToPb2o2nbYf0fXhW9g7yF9%2BeAxudZ7GUnGZOLKELZnQ4LAiqLFBWnByBSO%2BRFfJLd1MNMNaThcgGOqUBQW%2BvqPtv6A93LFSKH5DS6m0SjBtRwx62iico02Qp4z9GugHu1%2BjPoU4vxJN02td0xx7FA6ba5njP4NEvdKmijbtG0MZI1s7aOZ30vZo0kzklNdMBW4ors4gnRLeUbfx0e%2BCQAybDWEJ0azeNadn%2BChgZDgq9gW8e0oxvYyd1xvLPMwaVAbuEuZyipV50OzOlsgFp8KuSQU74o96g5patjTccbb2c&X-Amz-Signature=d6a0ea3570024f0d071b66e86d2481b26b6dd853ac6220fb71548617eea74eed&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

