---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XXPIUOXY%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T050049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCgiMnUogbzAOHsuAvbty6Rwg1aIo68zPlKWzKX8%2BAk4gIgRBuwrO0y7eIeL2w6hOR0Rk1ML4nn0fVVPWCBFyv2Bscq%2FwMIaRAAGgw2Mzc0MjMxODM4MDUiDH7DK%2BhFpQeX1vZoASrcA%2FwS14W23a4wIFzt1nitQXDu9UqDCCVlg2LsRUZ3iPDkLqQB5ZeVb8Ov6BiJJZFpnpdNhMYhNN5f8s%2F6yWHtw7Cb3eN2cuSSjC6iGCbjifge3YqHT%2FAwkuwWqxqU0tYNBNtMzc5KkMbyuwjIyffXH%2FwSnPTh%2F899HT2vO9iWdiOCpuHgnUG7a8ykNHpSVPUtVLqCvSNRIcon8O1G4fmlbUyLJEbwcvg%2Fr9V%2BAE1Kb5MQN%2Fugw2iPkHDrnrl%2FiuKx59KtGbr4T7%2FT6XsWwXKoaUoX3V2PoJziZiHnniqVRBAQQYO3O09Elm%2BvbmbUF9HvqVFvVSOyc1rPbZlrAJ2yiSFuSXcc7UbxrZqyjj6Kz3drOLXdN1v4IEmUJfp%2FVPqwPBCgk1i00%2BTJq6DUiFb3VaHaYtL8UQ2zFZkB3wEguzUFKJP1kENzPChGMCu5MZl11bgbAzqhE8%2B4u3Zkmd12MUHUfEB4IBJASZYYIqncOWpbFiiASBE5xoUSaRcGj2BIGaGzwUKvlRbxqowazL0YdOpKl7UTZ2rz5vWvSTRCtXx2ykFPk%2BBxsR%2BnzuI%2F%2B5O5MSA5JwZXCl%2F%2BHi8z8o6kwa3K6%2B9DLpRTOqcLKL2FxWOWOVzyPndg7uGTHrwZMO7hhscGOqUBpZ6wR6FVk9HnntZX2Mtkxcl87JzyTFGZiT45mWe4LQhzd4x%2FmzmlosFqqm9%2FhnC8LoeJWgiZiHr4Kfxf8LUf5WWZgttC2gM69TJTTpoXUBH848j1q%2BZVJbEIZNrbdIEQLM3NFZDNR3IlWQTEvI32gB5moPqx8Ugwuff4GuDlIHRL3%2FhoBjbm6DPjgfrEsOfXv5KJ4o6%2FdM3WiUM6fzk4tnMu3Eo0&X-Amz-Signature=e76ad06415dcbc87fcaaf2727567a4693866a0b059687acc82f74176fe4e08b5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

