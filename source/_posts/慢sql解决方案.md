---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665R5EUPBU%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T050053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDGvKslkBv2WhCdHwwvsVrd1VY6vQXpJrJe%2Fwq7gNhYbAIge%2BAWM3t9FGJTNUuFb%2FDFNVznBg6WZuSJGkPzJ7vaqecqiAQIhf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCmxtJsFs0Dbq0j9pyrcA2Y4kaX6WUedaoOKcbggh1%2F4uygOAnY9Xn0RGqAFcPTsDLpZnOcalIe%2FNXZIcGWEPEJ2JMpVFCl4vjNH0zD92RRedZQGYX2f8kRhC9UBVIpwlgnuFXxetJy35TPE2at2TJz8sFCh4wXrtwKbmy93iemp1CmLPMYKwFOSZ4NhFFNYUyDg%2FIu3swHjql7ypoWYt%2BZJME3xzh6SmYOu73Lx%2FYLP2M0MRyOzqBtJn9fKizrERNTBIYd3NwV5O7ABj0%2FDhueJp7W8JQZyqmRMkA2Xar3I0hbUelrNGsfkwcyExiSzTib89kBxNlduq0yo1RCIuxSvYUSpXa%2Fevj%2BteUddByuuZ0Rr%2FbuZXfMmvlyV%2FLSxnMMUtRsen5IkWBqhCFNaCVbCIbF8ZDvgrEp2zibCNOuP8T4Sy8MKFAvBAaxLWDlabtwI%2BKDqgwmHkmqJSf7MU4uY1A7J5uaVy%2BhxYWDRr3xfhRH6x%2BeTK2GlAyD6ICENmWi2IJVAKpyJCSgUvXlL0aydKYo9eVyduBMvxQKCcyoXLx3tJgMzmxxHLfFxpQ5wk6Rsry63IXcEAs%2FLhtM%2BTZEPEo%2FfzlAFpJ1uekjAO5nLhkaIvWDqU6Ez901D8J9YuxfkybVD0boptLVmMMzhwccGOqUBKiTVMO4GNFWnVyFVxhqcngH9tfn12ACGIYMsfInWA%2BhA2MmvIr3oBclBQPhSDFuS3GK3cjey7nszk2Rlj5ynyRI1Ln0twssJg3V5BhFNo9EdErP1yjZc7t2s2bUyW3wNCHi0qT%2Ba9IHHOfasaqZM3kWBcT5nYjgMz1S8zmSTcB6ai9vQftClSHSyOn0GV8oyi2MEJxg1q7DsPOzmqQosl6%2F5cHic&X-Amz-Signature=b5cbc50c14070b01b5e2521c326797b2c095988337b6b3cef08079db9d215154&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

