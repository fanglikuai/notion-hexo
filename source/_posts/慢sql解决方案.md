---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V2SJQ56K%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T010040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFmXuT1bSKbQQRV01cHKHNMynluxothAb%2BwhNDaknGPUAiEAtpLdvuMh%2F7%2BLJ8qBrHoeOnX9Q%2BKYxetoSwtHVJct92MqiAQIuv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKfpeLcuGC1FsWgZzyrcA9qNcl9IcfVVHD%2BUXi2CoSfJpxAZssW%2FEHYNhpqHkQiALt99fdpov9VOPb19n5p%2FujIEiWVraTyDda8o0BVrfsLQ%2Fr3aaXeX1W8fGkN33I%2FUzAd%2B9qgBZtZ1zxm1xHOdyxm6I%2Fxe5P9dGgleXYMNKMIQyMA10%2F%2BoFWHC2xyIN1XfvEcacNHfYDkZB5Tzcv7gFdjrJUWbbVc%2FAUIPGKMTyaJtCxbzOdJXOCavN%2BbR9KbJHvThwypdkZ%2F5D3Og%2BV%2BGmO3YHGi6dC0ra3LTmUcBQYRr6UO%2BREw0K9RIHN7BdXkmwD8YZyKCx6qyYdoiTOpIfGSEnDOlo4Nm5g7zMs%2B2UihAfa6IvqELG8py%2BOFmaJX0yD9icCkXIIQLUzDenElz9fjgCq%2FW%2BLqY6h25yxWQyqgWGJPR8t7EIImgPgaMJdTd3xQLyq6aI4WwxhoDelUGORIOjDBBbFOHtDYmu1DgcP6Sdji%2FBT79fwNyHp02YqS1WH9%2BgAP6Kxt6yo6MItJ798kYEETpa890peoK6zMr1tH%2BZ4xGUTa48DdlRf0RJuVKqFxEexvEvjskfLKgTtv8zayI8UF14iPMxxurxzrBg1sZUXWFHyxfThhGoMSsmaWRXn4GHJqsD0MNCgxUMLyA78gGOqUB8SkpVITrSGW%2BQG3WyEmJusUJ2LcBwjowMJLjod5RscLIbyim%2BL%2BgVrIv7GM4H%2F5WHkk7M2sj2ahUp9xtPgqozvhuBkyp4D7%2FTDrfR2YOydTgKxJ8oBmSeBsoTXi25Wv2yPHzF4XvzvByVBfFnVijJ6d7vwTzGv0RySTXKav9KZHtvTRTymInqqi%2B7yFVwHPkUu8lrr%2BolCpj5tCM9vDyEZGE0Uwy&X-Amz-Signature=3625794fa42e80bc257588395ea74b400ba67354645708eb957295574665185a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

