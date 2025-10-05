---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XXPIUOXY%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T050049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCgiMnUogbzAOHsuAvbty6Rwg1aIo68zPlKWzKX8%2BAk4gIgRBuwrO0y7eIeL2w6hOR0Rk1ML4nn0fVVPWCBFyv2Bscq%2FwMIaRAAGgw2Mzc0MjMxODM4MDUiDH7DK%2BhFpQeX1vZoASrcA%2FwS14W23a4wIFzt1nitQXDu9UqDCCVlg2LsRUZ3iPDkLqQB5ZeVb8Ov6BiJJZFpnpdNhMYhNN5f8s%2F6yWHtw7Cb3eN2cuSSjC6iGCbjifge3YqHT%2FAwkuwWqxqU0tYNBNtMzc5KkMbyuwjIyffXH%2FwSnPTh%2F899HT2vO9iWdiOCpuHgnUG7a8ykNHpSVPUtVLqCvSNRIcon8O1G4fmlbUyLJEbwcvg%2Fr9V%2BAE1Kb5MQN%2Fugw2iPkHDrnrl%2FiuKx59KtGbr4T7%2FT6XsWwXKoaUoX3V2PoJziZiHnniqVRBAQQYO3O09Elm%2BvbmbUF9HvqVFvVSOyc1rPbZlrAJ2yiSFuSXcc7UbxrZqyjj6Kz3drOLXdN1v4IEmUJfp%2FVPqwPBCgk1i00%2BTJq6DUiFb3VaHaYtL8UQ2zFZkB3wEguzUFKJP1kENzPChGMCu5MZl11bgbAzqhE8%2B4u3Zkmd12MUHUfEB4IBJASZYYIqncOWpbFiiASBE5xoUSaRcGj2BIGaGzwUKvlRbxqowazL0YdOpKl7UTZ2rz5vWvSTRCtXx2ykFPk%2BBxsR%2BnzuI%2F%2B5O5MSA5JwZXCl%2F%2BHi8z8o6kwa3K6%2B9DLpRTOqcLKL2FxWOWOVzyPndg7uGTHrwZMO7hhscGOqUBpZ6wR6FVk9HnntZX2Mtkxcl87JzyTFGZiT45mWe4LQhzd4x%2FmzmlosFqqm9%2FhnC8LoeJWgiZiHr4Kfxf8LUf5WWZgttC2gM69TJTTpoXUBH848j1q%2BZVJbEIZNrbdIEQLM3NFZDNR3IlWQTEvI32gB5moPqx8Ugwuff4GuDlIHRL3%2FhoBjbm6DPjgfrEsOfXv5KJ4o6%2FdM3WiUM6fzk4tnMu3Eo0&X-Amz-Signature=c9f41e76666df4e18b9f16ea17e11d163a7042eebbf532b18196b603200c4dfc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

