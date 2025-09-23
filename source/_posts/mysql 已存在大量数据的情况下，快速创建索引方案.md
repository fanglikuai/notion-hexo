---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WPPTK5YM%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T180043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDoJhT9n0yx%2BhcSjJePQ2yyS6233m%2FLywg0qT2yz0ctrQIgf2lPDFybb9yTXFuyAJ9eG5%2B%2F1O8jqJ9OaUpd1VEoYjMq%2FwMIShAAGgw2Mzc0MjMxODM4MDUiDDMRamQE2iiM1Yk3vSrcA%2BldcexHcvAd8SG0oVKPugDcnsieM9OB24QZfKTVE8CoMXb8brCB3D9Nm1OEDuUauapdr2JS3G9K1x%2FxkiXba7YWc9xHpIcnlOWbHmsPkQO2DDKWCbHEVs7yWhp9EBmbF2Ryxyx6Pq9gZoo1CsBbyhf88JGonYsC9rD1DuJLlQEtbWsIpsHo5exLmoITq5eQ%2F215Df3KtdZAbD0DrLUN0p76pgZGpaemA551WWiZTU1gG1bGl9tKWrGMEWYZJ4wQjccrc3u%2Fmhl2lXRb4X0kzZXsD0KpakkGcSwvRAj1txVOkQje80lAkjsacqfu7ECGO3z%2BVtksNthQLEmD4PDSSULgU%2BQRbYddynDteSw4MvI0SSEdrkS1uzhsma5n6CR07SYwo0xTN2SiMqachdll%2F3vFWzkJvDfHKylx9p6N7zlnFSGDiXKdP7JM7kYJgU7%2B6%2FFs7cc7wj7ltE%2FNSaK4P5A%2FAcxlq7g%2F1RVUHID0UXGdqDva71tQrXqtNXfXcSRzcxTRuBFLESDPSUvezpHk3s0anGJWtKKsAr8l6rrOz8XhtT0RRXb8%2BXLc4vA5fckC8Uh0Z6poy%2FzgCJimD8jXpexB8MUzYMcjgy6sOMAdiTA%2B%2BJ0ecB3j6YgYuq5VMJ6by8YGOqUBa2fy%2B%2FHs%2Bj421RMkAZ9WR41nHCFB4Jvmy%2F1o1%2F%2F4C0FbIid5HkLmEwunrvedANKHEuYQQZhHvMNXsvBb115Ba%2BMcEaH%2FYnlz7IdcVUZZCMwRSCN6Z4JkvKJcFsc5a7sTQRXEcoIISaXFT3RcaGxv9H%2FNOHy62QEIhcRbz%2BVLzTzSrxthBG1ir8MiRRkVYuDaqsJ9xkiRK7Gp9k%2F5m2bjvlYbemOX&X-Amz-Signature=bb16f2a5ccfad5e4f5ccbff1d6d92f719bc41a5619ff0eb496c855e6811160d5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

