---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UXBQP22H%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T030048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIENZM4j5yxwB74zFPmPQCksOziv%2Fcbll4wEEf9n%2Bg9EAAiAZU8c3iWUyLFpFsAKuXphbzvpO2CnPrG7WXyDBFkOtCCqIBAik%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMbAzPkPEqTPoWKbO%2FKtwDjMV20BAuFw6LGA0YNanyUys%2B8mkMMmlXssxQyB%2FvRfdWiJdbm6yxe8U2uvSKLYkiXu34la%2FyfIkqNIkHiQ5QYr2n9ToSmYdvQJu9VKAlrH%2B%2BbBQbpYUrVpEuCl97TZymwXlyrfr5YdpNBzDwmwYngeuGUfN6LPCYFBEES%2Ffa7c3u9btQykJzuRJce%2F13oLbfLEXrwl8WEZN2jfDAbey5kXKnZWYnWyQOMHgTdxzR4H7vJWL5ixCmyLE%2Fyuzvmyunwjz06IO1YeA89vUIt1dB%2F0YMfpVB5yJD8BBxcBZTMvWK%2BDPLtLdUkaivT3%2B%2F5iLJaU2jEc6QQ2ocwg0hjNYqrarh0EIcpW26f%2FFmsx%2F91F5T5BODOzTgvWREPdi8tjhsDFYGQBsqsCO5fq8j9QhYX5U%2Bqw36%2FQlv4Fb%2FpxbJbFPBZlNCIStgiI0%2BQ82%2B%2BX99Lh%2Btfe1DNd3Ip3zyTzCLc%2F4gn0rCX2gAnq0RN64%2BGrfodw%2FCyY4BDlkF7wOhvtTz%2FuEEt%2BV7MtdXOo9bbI4b%2FCTJddbm2O6GybeD7ishkKg6anfEKDw67AgRn4CwjYY13C6QMLC2S7sDfA27UpxGxO04KSDK0DA%2FW9tpHkAkjlWcibwB%2BWfhNlb%2BGccwqpLqyAY6pgFDoK70Z9ulJkbMYi%2F6h%2B%2BEudTo7dr7SXQn1MVYFftQ5xgnDREqCWYLxjKas6ihOrZIzntoYnnl4l8%2FDTh6YDRtae7LUTYpwNpvLcTu%2B%2BYUmmdTC%2FtfIjdX7n1zKwpxvHOU%2FGFKDfKrEnxG%2BuzYuT9S97YU5VFn%2BRlrhRP7GzDL5tsvvovEpGfaoGCCxncs8CZzJVCwRHvO5X0qqmJ699YC7nxqzBNV&X-Amz-Signature=fb3b4be5b575759182663b1b2aaea80a93b932b0a5b09c2a58d22ab66e41be71&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

