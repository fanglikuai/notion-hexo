---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W6LEGFTD%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T110045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCvUJhnbLvk3BjeHrzPf59%2FC25wEnT4zaOwCRR5NgLyQAIgQeWk3Fzv%2FvJxaJKSrVbChwagUOQthLhZqcurDChPTkYqiAQIkf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKROUxgH1YdTRX0t5CrcA%2FqTeYg59YOoZwwHzFtk8w213IzBOYfC6cIZNgRc1rb2mkOUHpuD%2BPEWhtHnp4lCN1ap6vbN4no4fkQbxqapELcY7Cvm2FvKQacETVUwOtZuPwHrYIXXDSm996DpQnjcH5V9u7sQsg9EjfimWaYNfD6iNJ4R0vPrI59VYxS6By8WH5eikrxI6xPay3OH0obaJCCrEIF11OGBiyYuxBZzQl%2FH62egfy0u7qHyVQcx7DqVMDN9YZMNM9gsTauDptHXgtu%2FpLCRr5u0YPW%2FQQuFOz8g8seHghua0XeP7zltyd9%2FZHOaYWXqs%2F8OSvrT%2FTv9zKtg9CTXVckUkxN5xGuIp7czrcA2meEVEhl8RuOGJu0di%2Fwp4mIpDwxwtnVPPN97lZKhrqLZq3lfZ3ThaSbM%2B0Pa%2FXTCeTq6pEzJ6jrnvFTPZgfF8afMxEJqeDNEKoXPL0ddmytDKEWR%2BdSTAUt3Al7zogknVZAXGHxxfj0VJS7c3DPtDMy8Vjpxto%2FyeYAqIhmHvyox4Sn7KB%2FOAwQ12D9lJRMV%2F3NEFSXn21nsFWNDK3%2FOfom%2BXBFjLFAknxzTjwius6mlisSPI%2B6X1H2fY6wTm%2Bn3RVBpVlJvSXjzKYJG8savqobtwdkaXo6QMP795cgGOqUBQFfdXt1%2FnPj7yzZqbzHu9tJ4CEuj4PaYYBxHiZjhMuZKoABbPeZUQeUdf7SL1xurza1lTa4BEux03HSx0M2IrNMX2dMoCHLjLMQipUGDTZfrxtx%2F2YEj8FFZ419jqUzeeZfFgWE5oEIAZpUYcJxbXfL5raBV1VhAL2u88rqgtbJgm9ruMT523t7NKh4EOBYMpjP17nt4PggJKd6%2BCRfTLOKganiL&X-Amz-Signature=4eb7f4858ccd1c113c5a20d8188bbd9ee4b1877f5268f520c215580ae0a82495&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

