---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664HHVP6G7%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T090047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFrW1qE0%2FeqEHiMHL7bTBaBTAj%2FVpJrI5D5OBzJ2RgdBAiEA4L6qDzi3r%2F5hpbI9SvFrKMuRbHOfcfK5ZxvPhOH0AO0q%2FwMIPhAAGgw2Mzc0MjMxODM4MDUiDBRPI9Xewkfu7VtehCrcA4dBZ5P1qgl9oIpdB8n30T4kDFK%2BXAapdCIsnQL%2Ba7Esnh1dtyQ5NL2xY%2B2fJXZJcgszTIDlcpZtQRITNcsYWnPOP2%2BYGqC7I838WPuAP8ja82lKCX3QcBHu0sdPPKJvv%2Fg8cDKHdlnyIMWrD6WtoSlYzu9%2B6gDqNJ5M5rAC43HJuY7aaOdb3nL2CXLkmran%2Fx%2Fe6CINldCYOFt5ns7X0hqKGzBq1w4hhiwrPuPtjk0jmHp65UrLpFBW7y9%2BqesfAewULvpjq%2BuKv%2F4JPifjL9kcri6UNlhp%2F%2FAef%2FUBPyT3Ug8x%2BH91VvSg6FhwPiSptPe7p%2BFUiU6d62POX6LWQG19UBsVJ9WlLsdEnf8HLRNXUAkccmdA9jh9C1PWpYHKiaZ2Mmy%2BGImWqOKq1Vi%2FrBUqghMa1zqqJ3ZLhkOpHQTAKW4%2FX7yEOvVLGllXIZ64K%2BXz31Mq8%2FTXfiOE5pH%2FDlF2tk2W3oXM2z%2FGdKfcY%2FEz0bLW3igFLkWYvXrMH7oaX1xhCMZ%2FXJFr1rhQFL6%2Bqa4auAf1tmmnVlSf0vYzDCo%2FEIVZxfhIBK%2BAbk4zuPFaDYG6fLICQo9jS%2FgVbMVI2BMPmn%2FrR%2Bbym493%2FlG879njTPil1N3h6BUWiryBMJas%2FcYGOqUB8PTaT2%2BfS3PoqF9lqPp2gs8bK2q2VeAa9jOqUbuZk%2BovEVQxUK%2BKHKYTzCee3aLPlF4Ca4Z84ZKPnWwO4gL2%2FIfxHmk5l6oXJUr0Oi6pthmwAGWea1ntNxLu2HaphRwpVUBAiMGJiOiflavsjFvT3PRYsNdRCaipwEctKeEsyoYQFQWwDz3wExT7Mv0KFLtgqdNcPfaObwvc0kUpKQ%2Be%2FrpMPcLB&X-Amz-Signature=27cf735571c3651ebbedd98aa3cae7854d8475f3fcd40ab2b36450597b9b0390&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

