---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666TRXVTIX%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T010053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD78vd986mDTwYniVb5C8Du6Rg1gN%2FaohLYWaobIUdLzQIgKz017NuC%2F9%2BIpdNDGpFK0AZZmE%2BvdU1YNGQpTxILmJQq%2FwMISRAAGgw2Mzc0MjMxODM4MDUiDBPSRZsRLoBPvPGTpSrcA1ZtboVYrZIZcUYOVtMjCR6aVo2TMj2Il4n9f%2Bxdy5%2Bg7eMl5GQ5zSuldOdnSgK35G%2FkB8KHAlGVwvA%2BXcyVnVoqPvwz%2B6fHM2T3p84VhD5qh7Qaj0hMwktJGAoflEaGC5%2Fdhbvh6nWpvifhbULe%2Fo2BSQBADCaRwLOc3r2tMt0f%2F5ORVsKLL0jl%2BxWW8i65alz7qLfa8MIyO1npO8oK6CbsKXerw1qpikLui72KLPa1H0obRPFxV6WQ5f40TtnuvkV2ys0wVGH7iM2aoRJra6%2BRqjtubSX1WMJLUY%2Bb%2Bc%2F4AD%2FwBbH15aNy6kwdqXaVimxmNxqluQvh8NBJyC19XYXbXlC%2FTrDAiBmizHLMImzkilkfP9Z%2FmHOeB8ETTWtPfv3c9ZWqyBSTXGsZni1itYIOOE%2Bq2scmGKRkhVNil9r9oMIfuHnB2HF3zHnCWn1YfCvOPj6R2KHMSXow6w8wSmA31TWAhMIUqrdaMrIbv8MJBL93FKlwOlmXoxr7T9CzG86IZblhDtDb4t2VOYHfsXE6%2BRma8qD30MKAf6T9%2FWwV2zxZ5WuNfMqaK%2FsD2FMNkkKP9aaTrOZi6JZaEkuVsXrsVIEp83mshsCIAynUthVxVUV0zlt8Eb8VAzUsMNLJjskGOqUBGPrVUqkmUPn8Ny1MjswVlr3RxqmE4ZdSKGyWdzMOMfIQOHemAm%2BV184vxyhDaIjN2QWSjxkJqR119OaS9BdHceKQQ1WDvnGrkpwiy%2BGCUlRoHzk1HnsfoHrIKZVzsrIdBmX7VrIbHpl%2BeiUYsIRMBI%2BuxcUzmIOHyXEywRP%2BiQYVlCWROkJ1U5jfEydvSFJk3giWpMJWbVfS6O5OOBXleb2KpwBc&X-Amz-Signature=05118073b6a78e6137603804c320c653d8696b5e40514ad1e5397dca34eb7c79&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

