---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R5J3JLLY%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T150054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAfsz%2FP8IMYDM1S0tVT6w6cThsLfyo1d3VQgbxsvou8AAiB3fzwDELj1HwxhyDuXXBv0PE%2FsfhPPcSZwGdcAQ7cQbyqIBAiw%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMQyMhW4Drt%2FENm8CLKtwDOI0p3IRDsd3odfBMU26nMMITG7YGBEr6xDcT2R8GkRMQaDxFee8NwKGoL3I35ulN4npjDROPXAQa%2FlaX4vQHtrB5Yj5jZ3sB%2BfqD9ul%2FDIh1nhSnIKH9%2BGRSXiv8ogXhr1NIYIOTpxCLfSXphRID8Zr3lGZ2BHjhwBdblC4YBi0KuXLQdwxj1TOHEzf9roEHXf0pUGI1pD6XX7NquTOJHZvQhcWQcpT9q0swRFlHAZaFBPJSxbK4WHJ%2F4qGLSpZEONdxGaB%2Bmmq4bNP7JdPhOg4BCHll3E%2BAoyD37EJo%2F9IDhW1yRk4VfmWWIhQ1I9aCG1Dy%2FTnJxKnH7n%2BqxXgge4XfV3L%2BJd2lit%2BQn%2BaXCnvnWgBCEeTThovARUkUUFGkCzuyfIvjjOH4jnTd7%2F%2Bg%2BGaa4941qaXsxzrDtwQp15ySnLfLu%2FdR%2FlvrgGwk0ksruAp9j3CwjJo2hUfPdQZURqkKByaDR6LpFtqJL5cI8mbquPamSbtgPcFGSUvDIkCg0XIEH%2FVEeFycMx%2Buo3Gt0iyofby%2FtHy%2BhqiKvyjA37GFRimDO%2FtYPCB%2Bd9NdnxamGJGgc0Li3qH72xli0kekGBcdfza0UMWgr0uhLMLHT8%2Bg2ujSBEGkssUX5O4w6ursyAY6pgH64S5qRtixcUfQ%2FFGWXDAf1u6jibOv30SrIDq7s2KtWQlwS0FRfE2STYpvthYn7aDJO1jTa7Z%2FBX3WiL%2BcbOrYSXSRtmk%2BUki6niQ2LoWbxYCc%2FH5BNjPZ5FdXAMP1m6t4zhDOq%2B76c2roAgvN03ciS0nb5a9olB5XwffvQfFag12YfoVjubwULqaBC1kFT2McdFU9bUMYzorHiGH8Z99BaQ2xYHRm&X-Amz-Signature=b5c817556aa9e71d88cb2771f909af4da7cd6cbfe93ece62b097396adbd311bd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

