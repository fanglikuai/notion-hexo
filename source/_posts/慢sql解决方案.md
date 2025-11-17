---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R5J3JLLY%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T150054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAfsz%2FP8IMYDM1S0tVT6w6cThsLfyo1d3VQgbxsvou8AAiB3fzwDELj1HwxhyDuXXBv0PE%2FsfhPPcSZwGdcAQ7cQbyqIBAiw%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMQyMhW4Drt%2FENm8CLKtwDOI0p3IRDsd3odfBMU26nMMITG7YGBEr6xDcT2R8GkRMQaDxFee8NwKGoL3I35ulN4npjDROPXAQa%2FlaX4vQHtrB5Yj5jZ3sB%2BfqD9ul%2FDIh1nhSnIKH9%2BGRSXiv8ogXhr1NIYIOTpxCLfSXphRID8Zr3lGZ2BHjhwBdblC4YBi0KuXLQdwxj1TOHEzf9roEHXf0pUGI1pD6XX7NquTOJHZvQhcWQcpT9q0swRFlHAZaFBPJSxbK4WHJ%2F4qGLSpZEONdxGaB%2Bmmq4bNP7JdPhOg4BCHll3E%2BAoyD37EJo%2F9IDhW1yRk4VfmWWIhQ1I9aCG1Dy%2FTnJxKnH7n%2BqxXgge4XfV3L%2BJd2lit%2BQn%2BaXCnvnWgBCEeTThovARUkUUFGkCzuyfIvjjOH4jnTd7%2F%2Bg%2BGaa4941qaXsxzrDtwQp15ySnLfLu%2FdR%2FlvrgGwk0ksruAp9j3CwjJo2hUfPdQZURqkKByaDR6LpFtqJL5cI8mbquPamSbtgPcFGSUvDIkCg0XIEH%2FVEeFycMx%2Buo3Gt0iyofby%2FtHy%2BhqiKvyjA37GFRimDO%2FtYPCB%2Bd9NdnxamGJGgc0Li3qH72xli0kekGBcdfza0UMWgr0uhLMLHT8%2Bg2ujSBEGkssUX5O4w6ursyAY6pgH64S5qRtixcUfQ%2FFGWXDAf1u6jibOv30SrIDq7s2KtWQlwS0FRfE2STYpvthYn7aDJO1jTa7Z%2FBX3WiL%2BcbOrYSXSRtmk%2BUki6niQ2LoWbxYCc%2FH5BNjPZ5FdXAMP1m6t4zhDOq%2B76c2roAgvN03ciS0nb5a9olB5XwffvQfFag12YfoVjubwULqaBC1kFT2McdFU9bUMYzorHiGH8Z99BaQ2xYHRm&X-Amz-Signature=ac6951f18e652d5180958c594bc289082f6374bc4603b8bac5ef5d849e412a12&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

