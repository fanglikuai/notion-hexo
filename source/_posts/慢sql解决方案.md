---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466553NW4HO%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T030039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC9VpQvNWO2C0olY%2FBYo95SDb6E0JFflHhOkK6sLL9d5wIgSV%2B0E1Ekqc9Xf5GMXkERBrO5RPZRGtiBkEmC%2BetTQzkq%2FwMIUhAAGgw2Mzc0MjMxODM4MDUiDO791ziZTKIwLe4hkyrcA8JjtqmzzX9psUivECqrsrktVne8eIRb8gRwBeuwHr9l8c6oaxTkJ5TEmIHbzKpQpIxa5NByp99ppqZk%2F9JV1d0WKWgvEAZLgafUxW9t%2BVTLtPSQ5e1%2Fke8HjtRBU%2Bs5l9S01MLM%2BtFO%2BU%2BNC35n3gaCfGQQXMjgY0WveyN9E1szSZYwSpvx1vsAFcGWJZ0zFJQYmWzM9e%2FzTHhGAhARZ4CpOUV7Ahf9NIDgS0tDsBgGnrw%2F%2BxTkoZaZy1r3Hq6raSMO0XIoW0EbZ%2BIaaoB5DhzqmHmyBg3T8sfroqMqHvwkmKw4d6TlkQiEihaH6UcTwHUnr%2BvKEm1LRG%2FbB%2F0P5gB2Uriwdo6WQ%2BTHNasUxgoLxZDA%2BWVRu6MTYWOuLOq8L24%2Fky6HEYtbg8qW3BM7OjJ6Hz6S%2B0TaJsx1m8P8X69iUUO01%2FK8QNaStsbkfereV5PUc%2BhAU9BDJc%2Bjt84XweokQL%2FEYhnNJ%2BNy50xURX6YNFBeFAGBUPIhC%2B%2FtRuZBZxALlTZvxtuKVwCWWx%2BSZ1F7oN4hq1W8xdMZ9MYMTrQQ5hFX6UjP1TpDMRjressXOFvQ93XbtU5OUihYdVuOfJNJXV3l%2FzrTQCQPUAeHfy1YtKlP7FFFrA1MxON6MNjyn8gGOqUBkzyCWc%2Br0JjHYY%2FKOdhk3lncO7TDtmXgb%2Fy0R09mooPfA8uDkXGkxsTMSKIdxNGNdwm3N4q%2FwsGLwWtLx0WzvaT87yVc47B%2FIbPjlvuXn2hqBrx7KKSO%2F6717qDQbwEZwqoqiTOXkRZ05JSI9%2F%2Fbm8vvEwX5%2FJYjldfdo%2FZmtEMAFcTrqDnHTZ0gQJxoft2Jf12Vgy6qjUDitrToxyAMC4x2txDf&X-Amz-Signature=93ed3e6471519fab3d796efbde345754e61b42dbf3a491ce8c2e7b59feeecddb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

