---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SD2PXNBI%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T150052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIC%2BtVpCqAvIl6XSv5DfSFyxZCkapfXHfC20%2BTZu0HNlDAiEA8YPaLANk2YiUl27ciqGF%2FqO8zKq60wY%2FpTD4X6rbpOIqiAQIkP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDE8KPXfigN75kCT4YCrcA1HNiE4EzPOE8HRikVzU3YPpk3%2BPl7CQh1LgLLSHNJRPxaY47aNzgYEkUNqW1N9GN4n7mKHBri37bOlnBrToG3tf7CtkoKYWFp6z%2B5bO1sIkyRHWkzjEJ6WQG4tTgVAE%2Fwz0uHBaD2R%2Fh0Ht91PyLVVixNybgweHBjmuJNvDsdO7xgQZja9TPfvyZ5rvu4bb%2FE3F%2FJFFprzyVoxzG7Cb5Z0DXWwj5x4UgMwkAn1Ey2s%2FJVbTCjoY0Y2wWW0PT%2BsS9%2BfhemnYnbQ0AZZKjBZW%2B%2BYiFvSk7%2BSyFJRHAZvgNAKaDWi2nrxOxBhMJ%2Bx6D92cd51uCDTAaBSbxhJbbTqRGAaX6RyvBmuXg6Q6Po5wIknmdVfV1oQQTqOgQ94WP0xpADqqQIPc6L2xDTIQSb7es9LVNnO8pdQZ0XJyVtjHymye%2FvfmvM7Fv0pSVL14XugY6XLmpvirNKt%2BZkTJNrpkobe72bGmHhl%2BjwqB39m0fXO3jXXVIZDROnG%2FJ9tOwQxghraQY1oR0dc46%2F6gjgdtuv822F9o2CnAi%2BJHMc2Kdeo2GEePlK3hrP%2F4XYPgBFZQ%2Fnjn08bw9TnDOnhkb3pvTvl6hnVeBA8Z%2F6YnPN9gD3%2B3ysupCg8s7kDaiht6MMiKxMcGOqUBm6F5aLBV4LBFYJ%2FH1YAMC0ZwfDg%2BAwBOPYaxfwcKpMYEIxLUfCcZLVFqmTz5YNGWKHx6q4C%2FDJj20qTEBuRPZUl9s1KokCXiIPxmAv3rGDTtLD%2BxtocWPt5iEB%2FeVw6elhVkDbTRN04ZRZ02sICOntzgFCWJRwD9lmIYTbhY4PL8cRktqhAX%2BaiQZ0PxfvhjzIQkfs%2FSUl%2B0TTxl5500PWLKNjAO&X-Amz-Signature=49f11b194726cd9ee0f61286f9b78601c01859399727e030c8c1213178e62b98&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

