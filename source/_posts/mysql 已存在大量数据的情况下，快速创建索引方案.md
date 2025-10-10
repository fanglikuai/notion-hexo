---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666AVW4OHE%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T070100Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJHMEUCIQDv2%2Fyek8DON%2BN6J5%2Fz1bXEKu1V7OQ5UOPXSt56VyIGmwIgCf1wWVfwc%2F%2FxAdkaYFIVJ9C9pLTOubArYt309khUoKEqiAQI6P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOSVhYxWJU6q1Fz5FircA53hvJm0RZ%2Fa%2FUT3JCPLtfhapYLjalOsqDKeB8FwrD6MgUmgbls3Ng5MHaAdYALm4nrI%2BNjRRzDS4q7KDYlrE1Q9hmbGJXxEdQrZvsT5tyLwTSPCeSqa%2FlNxv%2B6LnzHQ98RJ8%2B0rbuAjd5ipgc7evZWxJG8owJ83s5A0LGA7snxIHJ7sCqrcKinJ6P2O6CLHvuyjPYBBwp295zOtpGZaC7BY9YWPTYTMZlQGitp9fxAbnIqAn2UeEDFkwbKhM4Ej37iAKlWGW8xtXFLpTnY6v2W7z0HRlIhtTQlflmSIFuQqPpIak%2F%2B%2Fe0xu1YP0XElr9v7NOdEA6TMCglYZUVWVXUTYiM7Pmo9FMDTzIT7KGQCsaFzEsbVsxbkEkJB%2FciVgXOaRDjjwLwH%2B02cEFXKN7mAXYtcLmj3gOC%2BAVZm95Ok16CLzu3vXVFwU0kyPQTXvxwVze93JtXxTNCOx%2B8z37H6cVWEVtF4s1zXeJAGOhNaQtWvGAi8nDlXmyU0tFXKvrp2tQilTJW%2BlO%2F9tac86nin6Rh8mLNbvf4Bmeb2h0fvpCZGavhNrkoou1beP%2FxSvEueKCMCRtm2sL9WkA6yTsCmRRLlvKWc2jONOOFW%2FSMnr1i90W7rELFxO3gnRMIDaoscGOqUBR7ZygwP3Alq1FacTXD8%2BzRFPRP28ahA6PqBCQjiIQMAN8yKCgJ1Gk523onbhd3GaR0Qw18pNGbr9zo9kRh2BfuJi5gNsLvUyDM%2B8Hmb4860BzYBMHlIbCZRzUtxFp4cz64umEtqfZks9iFHsrruqjnQ%2FMZBFy0brQhjdsKo8KZo%2BgHK2aT5tgJrzGhnmbF8Bg5c6jWgI8wO55QX74y7aW4niBbhs&X-Amz-Signature=50f6e6b0edb9e81943d474630ff47788b88d1bd08f6fc6c69c255a85dd59154b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

