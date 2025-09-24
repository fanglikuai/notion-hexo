---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664C25V23L%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T140120Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDLySIwcxum8w056%2Fq55OMV%2FwbDkxawzAhcG0JA7hIakQIgROPVBeREInQ5UWTSs40PEJR71vpGMpNgw3DNfBabb7cq%2FwMIXhAAGgw2Mzc0MjMxODM4MDUiDEW0iUTDlN6ijlhxCyrcAzykKFvNKl2zCRMYRwhxsNFHkBtzR028lhZS%2FZMS7oyczOERuxq5A1IV7Zg%2B%2F7Rpvc688SIYrFQNqA93tRqOj0D%2BS3IMfdGE0iOExzTTClGXAS5INUdqiSq9Lj%2BslvL5koFa%2BTbtc7zVVKXS5YAX3gzZVZyKMBY2NoZd7Dm9YKFv%2BYaVqmJzDE5k3BcOs2buDKpyhjqEZcrmcQoWGUNP0HrIqafOERUU33pxilDWNwZwyDxx8oKn3WMRYp0l1dCyID8wVvxyl24v7m9kS7%2FCD7Yd8b44QgXM5eyBo5a0mYbe7GdVC2UbdMgDa6l7Qtz2pi72ngVoc%2B0XljOz9fN54TjpR90mJZgahBgBYAbLH%2BeqK2nM3HIi4xiLx%2FaqFTrWz6Dn3J3CfJdyVyVyOVsCOXtzXso9xt5J0orvlkq5j5K8vXJvSV4j%2BvkA32h%2BLmvVGUihyu9kPzbPogSFaMSHwcxNwW59%2ByVxekpzMx31SzQNJcRqQSCr8SlZkQ2gfugnCIqkEazYz1ea68nJw3%2FLYiI%2B2GD6kewyxeqgS6nCqRhOj6v%2BzJ1l2B59c1JNlNZ5JHxMzE9RocPCNouczS%2BguVfei9qDMYJ%2BlbZLCzdLRtdnTQHHmYL%2Fi7YkYviPMJvSz8YGOqUBRgWmLzNbxcaWh%2Fu1obMhez6hVZrmxIc%2Bd0EFyZbuAr0umTCCDjBaqwTwfd%2ByEvuX8tACNRLJN1E60PH6uOngSfDCH3C2biIdyyWGtGwnZUPPPlUX37XdUZNqYXvPFSKTAJdLEwaW6wOd0%2FzPzSw674SOe%2FNTM3bYkLMAyYBm1c46EaiZksHTPVZQKJ4OcEVu1B9TewfjW1Yc4Tr7LFn30RFXL5ot&X-Amz-Signature=b898949cbd589363dbf90ecf062cdc2500702a51a795989b8e709c41548d869a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

