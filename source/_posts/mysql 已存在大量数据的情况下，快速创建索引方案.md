---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S6K6MPPR%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T220046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAYaCXVzLXdlc3QtMiJGMEQCIGInNWSyW7qr6KRtdOtNexhTWrPwktpMyJsHvHvj2HGAAiAn7BJu4sakucRPYhvsKVifVN5NE2Rc4pylZxuhrsccayqIBAiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMFMOfTG%2FGLvnafB4NKtwDo5BqB7PS49HFd2oZZtfTSlWAH1LEn5VRJEHKQPCfRwKWapdEquiJjT26n2bInBYGIRG5R7gtyiLOxs1WkdeKB05PoVeUYs03ejVyhRTJimnd9aH7Musienxg%2BAArCiu71kxNg7TjP%2BQZL6AFYLR%2FeQM%2FKoZvLAScvjcW0mkFX8GiTtMe6iHtdMGl5OKmdECmh%2FRJCeOQ0F5dFryHrnrh%2BcB8Nqf1hFd0jwSCz0CdDjAYOUCJ5IpEhvUoa85Gna7v%2BGw8XkV4vG%2FmK14oeVDR%2B%2B8ieMVOUTs7b5WFGJ7zkg5MT5Dm3eIaLf1GfBi3ojwlUcioN8X3Rb%2BekqVCbL4q%2FDhnLGDKICT6OzK4Rat5%2FaBlXLvdgDj%2BOWWpYOOMKGa1CKpY3uhl2BCzgnWBiKTCGQhPZ49lWn4LdhdOl%2FMPmnFWAuV0T4plELpnliE48bYd5G%2BYBpp%2B3iV7ZMz0U%2F7RNcio4tC7kxwQPJ0RpSZpfDXvePP4MG60oN7iyEPyHXFfnFWfLpDeXnH6yf6Omuw1MJXvQG%2BN6mRh1udOH57DfbDo%2FYi2oDmaRga1l6wmwnZxEBqJT%2BSUXbqFAiJ0JZl27v8FJS8Ec1PtHbb5OvGIoGaz1bSmKA%2Bgy9tVjgkwhvjKxwY6pgGKhl%2BBI0vDR5HaOVSdS40ScIt%2BtcHJ%2FhiHz0a4RzBWFTnZPlv%2B6R3yKiep0TlJVSGntfNstSScPR2KafzhTELZJakuJRW1VYy4VUBbOEJXXvKNYsms%2BFmlnocxpSKMQu4ylVcD8mow0UF8bLsYJ9wsal4mRF4i2Uhl4ngdFKtGatzBRUsnSAjvr3FHGePAH%2BEqsVdmuJv10wo%2F1sGuovAKf5WXNd8v&X-Amz-Signature=4c763d4b620dcc2c4eade6956759eef069a8f0053e06ff75525913a8253abecc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

