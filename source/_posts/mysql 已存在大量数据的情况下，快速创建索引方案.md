---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SD2PXNBI%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T150052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIC%2BtVpCqAvIl6XSv5DfSFyxZCkapfXHfC20%2BTZu0HNlDAiEA8YPaLANk2YiUl27ciqGF%2FqO8zKq60wY%2FpTD4X6rbpOIqiAQIkP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDE8KPXfigN75kCT4YCrcA1HNiE4EzPOE8HRikVzU3YPpk3%2BPl7CQh1LgLLSHNJRPxaY47aNzgYEkUNqW1N9GN4n7mKHBri37bOlnBrToG3tf7CtkoKYWFp6z%2B5bO1sIkyRHWkzjEJ6WQG4tTgVAE%2Fwz0uHBaD2R%2Fh0Ht91PyLVVixNybgweHBjmuJNvDsdO7xgQZja9TPfvyZ5rvu4bb%2FE3F%2FJFFprzyVoxzG7Cb5Z0DXWwj5x4UgMwkAn1Ey2s%2FJVbTCjoY0Y2wWW0PT%2BsS9%2BfhemnYnbQ0AZZKjBZW%2B%2BYiFvSk7%2BSyFJRHAZvgNAKaDWi2nrxOxBhMJ%2Bx6D92cd51uCDTAaBSbxhJbbTqRGAaX6RyvBmuXg6Q6Po5wIknmdVfV1oQQTqOgQ94WP0xpADqqQIPc6L2xDTIQSb7es9LVNnO8pdQZ0XJyVtjHymye%2FvfmvM7Fv0pSVL14XugY6XLmpvirNKt%2BZkTJNrpkobe72bGmHhl%2BjwqB39m0fXO3jXXVIZDROnG%2FJ9tOwQxghraQY1oR0dc46%2F6gjgdtuv822F9o2CnAi%2BJHMc2Kdeo2GEePlK3hrP%2F4XYPgBFZQ%2Fnjn08bw9TnDOnhkb3pvTvl6hnVeBA8Z%2F6YnPN9gD3%2B3ysupCg8s7kDaiht6MMiKxMcGOqUBm6F5aLBV4LBFYJ%2FH1YAMC0ZwfDg%2BAwBOPYaxfwcKpMYEIxLUfCcZLVFqmTz5YNGWKHx6q4C%2FDJj20qTEBuRPZUl9s1KokCXiIPxmAv3rGDTtLD%2BxtocWPt5iEB%2FeVw6elhVkDbTRN04ZRZ02sICOntzgFCWJRwD9lmIYTbhY4PL8cRktqhAX%2BaiQZ0PxfvhjzIQkfs%2FSUl%2B0TTxl5500PWLKNjAO&X-Amz-Signature=1ad3837c995382ed5e04902d7ea5a9db4cb2788e7f2793f5edca4015a17649b0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

