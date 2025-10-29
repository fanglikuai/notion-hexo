---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665SJL4URA%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T010039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJHMEUCIQCmSwusZmUbuFTmMwF1qwchWCNTe5Wtg0ZSUH0pKNMuTAIgYRY7QJBL26G%2FGyfmR0Vhvx7DIS4tOQqEpVAZCwYx4XgqiAQIyP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOxBqZYd%2BzeeM2iJcircA51b0vYWWhaYkD7PoAP%2BaeRa2J2h4JeTLl%2BKhTHVvS2afXlLdJcTNi01sdYxvnGPfMR03kN9D24t4IIVv2Le%2F4zROpcmxDlvKhBhnuu3ze5hHseEZZFp2F8SnnjnMDdIcMZqyghSWuDY%2FAXJX11BZOQxwJ7Q1AS2nKdcs%2BTO%2F9VekSWefseftUWe7NCJ2JeT7HiUxx96qxVqux2UJQVt3fciKwkOEWaaFYhaO7gz6GhY8Jqq2dQAsd%2FYfgTJTin8DiZc%2BIMFEBJ40XfvJVIkC%2BHyV%2BVBNW1DkoPlFXfEMSeJFtaU4cShVcqMgwCMFxWrGbd5RYM7J1Tod5t5N0Eh36QXG5hweNJi2san2YA7MBC7%2Fs7E5rKVm39Cibly1ca3tsLZXLTH%2FNpeNK5UEqLT%2BkvuUed%2BPhEmTsNGr%2F1iJt2AQEtuHJmMAs5E0qHsLRmbJxnDQStTVJV2UQcVLxsrquDxh%2FAH8VtM1%2BBBGqs86hR76KncTohL7s3SwrEMs0CDkOKQl1%2BudEiYh%2FhwbhAYUgZ6eo1Q17nd3JJDClitoRwwerd7SK%2FSCIOSToPb2o2nbYf0fXhW9g7yF9%2BeAxudZ7GUnGZOLKELZnQ4LAiqLFBWnByBSO%2BRFfJLd1MNMNaThcgGOqUBQW%2BvqPtv6A93LFSKH5DS6m0SjBtRwx62iico02Qp4z9GugHu1%2BjPoU4vxJN02td0xx7FA6ba5njP4NEvdKmijbtG0MZI1s7aOZ30vZo0kzklNdMBW4ors4gnRLeUbfx0e%2BCQAybDWEJ0azeNadn%2BChgZDgq9gW8e0oxvYyd1xvLPMwaVAbuEuZyipV50OzOlsgFp8KuSQU74o96g5patjTccbb2c&X-Amz-Signature=14112090e6e96a6ca42d5ac81c2edc1db07501bc831a6ab7d186e155dc49986e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

