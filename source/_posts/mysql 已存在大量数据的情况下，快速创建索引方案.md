---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665OGITHNK%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T100044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBEaCXVzLXdlc3QtMiJGMEQCIANyA1yRJ68WUNt1LMap5G48bmFgLkjmgsWGed5BvEm9AiBQZNcr0fSi2ADmHzuJFEflSgBSAcnqzKXPWKQ1hCLktyqIBAja%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMhI6rjEV4dND0qVu4KtwDLM88VCkqavVtBWKHF7KW38w6HTDJ1R4rGw1IWPn060G9NjuH4aFTiBI386zDE4m5w6uDdmUYOM2TAPfOpxcRG83ImjDozD1g1t%2BJbbb%2F07Jk16IUtD7X1nLmoOcqBhH0UaT7mJbXk6oh7I3e2Qe4lyYd%2FUzTu2FgYGIL8NA%2BDOP8uTooMEJof9gvVBL6pgXQifc8DmyqRS6LApJsRuaiukC5phPr1MNa7JGH9FF2x%2Bd7qZRmAEERQRrZWIiEI6%2B8W5ec8Cw%2FX0wMUBGMu7Onf0c%2BHpQM3JxrjP%2Fd9%2FBby0WTZO%2FpDdfE9knEGIfrPw8Y%2Bk2UTRt7Tf4ctD89wI7IgqIvX21ZaWf75lSWMQI5IZUAKx5gfcc%2Fv7U95XiJT610oVsNXe28vGVhnlnYxkyZe1QVZSsgpEtQeaE%2FIEYXJovHSPhDrqgNRghAcNxjQUiWHRleoJlnmbCVKPlmH6OnKaYX4usbxPBpEjs488LxBLIC3HjfaDpEfNLhIIOff9bTK3RHZB3tAkb2G6k0MxrJz7JQBkmBnK8NxWXVuz9OZjJatlJjiz4OunWjI8jUhQ9luY5DGPJ%2F6umT3EQ0vHU6OTAW5xJA2En3wUoE%2Fmv1Pdz3o2GfjDfOkboTLHUwnZb2yAY6pgECTIC3WaiwPD5HLb3NWrGrRo%2Fdcf2hc9KDSixP8kY7q4yFJj1Mvs10QdXsyIX0vYrIP2PK2HXi7ZBRrUJWGBk1JwMvhTeYKpk5lAJeHEmIkqcZ4TRR0lympKlzjLCnvAPdWJjnlh8mFR4k2PN84IaOmS67qvXz8kcQDJGBrLO5caABqqyMGdoKtf1TG3htP2vrsMXesV8%2BrOHXt7IIwYAfTQN2ODYR&X-Amz-Signature=1d16cfdd1d23e319f16d207d289b9f722ed5f51a323b414e4ef6ebb8b20e1216&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

