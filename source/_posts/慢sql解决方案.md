---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WPPTK5YM%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T180043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDoJhT9n0yx%2BhcSjJePQ2yyS6233m%2FLywg0qT2yz0ctrQIgf2lPDFybb9yTXFuyAJ9eG5%2B%2F1O8jqJ9OaUpd1VEoYjMq%2FwMIShAAGgw2Mzc0MjMxODM4MDUiDDMRamQE2iiM1Yk3vSrcA%2BldcexHcvAd8SG0oVKPugDcnsieM9OB24QZfKTVE8CoMXb8brCB3D9Nm1OEDuUauapdr2JS3G9K1x%2FxkiXba7YWc9xHpIcnlOWbHmsPkQO2DDKWCbHEVs7yWhp9EBmbF2Ryxyx6Pq9gZoo1CsBbyhf88JGonYsC9rD1DuJLlQEtbWsIpsHo5exLmoITq5eQ%2F215Df3KtdZAbD0DrLUN0p76pgZGpaemA551WWiZTU1gG1bGl9tKWrGMEWYZJ4wQjccrc3u%2Fmhl2lXRb4X0kzZXsD0KpakkGcSwvRAj1txVOkQje80lAkjsacqfu7ECGO3z%2BVtksNthQLEmD4PDSSULgU%2BQRbYddynDteSw4MvI0SSEdrkS1uzhsma5n6CR07SYwo0xTN2SiMqachdll%2F3vFWzkJvDfHKylx9p6N7zlnFSGDiXKdP7JM7kYJgU7%2B6%2FFs7cc7wj7ltE%2FNSaK4P5A%2FAcxlq7g%2F1RVUHID0UXGdqDva71tQrXqtNXfXcSRzcxTRuBFLESDPSUvezpHk3s0anGJWtKKsAr8l6rrOz8XhtT0RRXb8%2BXLc4vA5fckC8Uh0Z6poy%2FzgCJimD8jXpexB8MUzYMcjgy6sOMAdiTA%2B%2BJ0ecB3j6YgYuq5VMJ6by8YGOqUBa2fy%2B%2FHs%2Bj421RMkAZ9WR41nHCFB4Jvmy%2F1o1%2F%2F4C0FbIid5HkLmEwunrvedANKHEuYQQZhHvMNXsvBb115Ba%2BMcEaH%2FYnlz7IdcVUZZCMwRSCN6Z4JkvKJcFsc5a7sTQRXEcoIISaXFT3RcaGxv9H%2FNOHy62QEIhcRbz%2BVLzTzSrxthBG1ir8MiRRkVYuDaqsJ9xkiRK7Gp9k%2F5m2bjvlYbemOX&X-Amz-Signature=da0e39d1cc6d15b5e79e3ba38b41069fd8c4371bd11ef1321b1ffa0842c4e9e0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

