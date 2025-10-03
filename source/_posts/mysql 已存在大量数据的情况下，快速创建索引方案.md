---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664HHVP6G7%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T090047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFrW1qE0%2FeqEHiMHL7bTBaBTAj%2FVpJrI5D5OBzJ2RgdBAiEA4L6qDzi3r%2F5hpbI9SvFrKMuRbHOfcfK5ZxvPhOH0AO0q%2FwMIPhAAGgw2Mzc0MjMxODM4MDUiDBRPI9Xewkfu7VtehCrcA4dBZ5P1qgl9oIpdB8n30T4kDFK%2BXAapdCIsnQL%2Ba7Esnh1dtyQ5NL2xY%2B2fJXZJcgszTIDlcpZtQRITNcsYWnPOP2%2BYGqC7I838WPuAP8ja82lKCX3QcBHu0sdPPKJvv%2Fg8cDKHdlnyIMWrD6WtoSlYzu9%2B6gDqNJ5M5rAC43HJuY7aaOdb3nL2CXLkmran%2Fx%2Fe6CINldCYOFt5ns7X0hqKGzBq1w4hhiwrPuPtjk0jmHp65UrLpFBW7y9%2BqesfAewULvpjq%2BuKv%2F4JPifjL9kcri6UNlhp%2F%2FAef%2FUBPyT3Ug8x%2BH91VvSg6FhwPiSptPe7p%2BFUiU6d62POX6LWQG19UBsVJ9WlLsdEnf8HLRNXUAkccmdA9jh9C1PWpYHKiaZ2Mmy%2BGImWqOKq1Vi%2FrBUqghMa1zqqJ3ZLhkOpHQTAKW4%2FX7yEOvVLGllXIZ64K%2BXz31Mq8%2FTXfiOE5pH%2FDlF2tk2W3oXM2z%2FGdKfcY%2FEz0bLW3igFLkWYvXrMH7oaX1xhCMZ%2FXJFr1rhQFL6%2Bqa4auAf1tmmnVlSf0vYzDCo%2FEIVZxfhIBK%2BAbk4zuPFaDYG6fLICQo9jS%2FgVbMVI2BMPmn%2FrR%2Bbym493%2FlG879njTPil1N3h6BUWiryBMJas%2FcYGOqUB8PTaT2%2BfS3PoqF9lqPp2gs8bK2q2VeAa9jOqUbuZk%2BovEVQxUK%2BKHKYTzCee3aLPlF4Ca4Z84ZKPnWwO4gL2%2FIfxHmk5l6oXJUr0Oi6pthmwAGWea1ntNxLu2HaphRwpVUBAiMGJiOiflavsjFvT3PRYsNdRCaipwEctKeEsyoYQFQWwDz3wExT7Mv0KFLtgqdNcPfaObwvc0kUpKQ%2Be%2FrpMPcLB&X-Amz-Signature=eaf01c7595d59ee417b9453001f027f0276594f5058d72214d28ff43f31a342e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

