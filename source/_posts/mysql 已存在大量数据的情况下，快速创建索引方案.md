---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TD7I3COD%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T220045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH0aCXVzLXdlc3QtMiJHMEUCIAXb9hd0Wr6KhClvz6kaD7A2VIs5D4GOZlAx3zJWo7hQAiEA4nQ0inw2Nh455Pl0eQ%2FiI1dxk%2FX4NyE5isUsnE%2B5tBcq%2FwMINRAAGgw2Mzc0MjMxODM4MDUiDCqQtq8IVPg37W%2FHzCrcA2tmUKIP9uCkofkppnjnihkTzv2alZ00VdEDK2ciUvGZLofZtbjA6oC7%2F5pYdNRFxzcI5vsgupziecbvgUytmCiKcZoMe2xaK7SQ3OCdu%2B6C8sdvTsPG7CipFeIpBPvrfr2kbwhda2i3vfhJy9C27pcKOhrLhahi%2Ffu2i1xwVwYAh8tGQRjnV24NwxWNAg5A%2FwFBCsWA8EbRAPS8i5y0v0qlwt07l5sL98ndGxvVAGGf7yULgo%2Fg%2FrAxt9k7ZKLhay3VqPvXjFX6d22xG8%2F64n4cJvbywvhnrC%2BTysCXDCu8srn0S90SwEoJd0rKH6s4arcnwMhkI%2B9868L%2BgX6uUz20zcwgTy5ZW9aaWw1IXdAyi2Kd7iLiDQ%2Fr7gbw7OwEpiqtbuEbrQc1yoYKqIdyD3Q6RztljgzzfimQXgb2I012lHCcSQU5N3QSYoOn5hXWdRheyWRZqewIDxZogy9T9hyXpIE7r9BW8sTrDdCPgEyztAIyO4arSPrs%2FqEkK0Fskg5wU8Zv241z6pHdT1nX2kSWKgqPBnPwO76V0nxMwTyLbd85zO5jTTTlKQe30FhHBfWMFxuRJoxnINqHkIbxyRC4CjzSMy%2Bp7zBBcH17VuosQL6IQmNcm1MQ%2Bz5oMPf45McGOqUBoYDcAyt8vBtpDnufydpgBmf06iDKkJ%2BwG4vDzg3IQsVK4XPOyNzE8ENSrSxAQXSnHzl0klO6S%2Ft6GiLN53WJr3uwgsVnIU3qVIF0tcRnTwxPseauWCJqSzIGlcYQXAEF1X7kocz%2FTlnPQqUv4p5l3C39%2BdpZ%2FYkuKsZfMB9O9U%2FvkhO2dfPRWVeWihLEHb2Qas3PAiYvpwtuhOCkpv84Y7wHt9Wp&X-Amz-Signature=ca057af11d42291678266d05acf7e1fc1123b93a27cecfc780f6a9c7ee22971b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

