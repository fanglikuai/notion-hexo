---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S6K6MPPR%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T220046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAYaCXVzLXdlc3QtMiJGMEQCIGInNWSyW7qr6KRtdOtNexhTWrPwktpMyJsHvHvj2HGAAiAn7BJu4sakucRPYhvsKVifVN5NE2Rc4pylZxuhrsccayqIBAiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMFMOfTG%2FGLvnafB4NKtwDo5BqB7PS49HFd2oZZtfTSlWAH1LEn5VRJEHKQPCfRwKWapdEquiJjT26n2bInBYGIRG5R7gtyiLOxs1WkdeKB05PoVeUYs03ejVyhRTJimnd9aH7Musienxg%2BAArCiu71kxNg7TjP%2BQZL6AFYLR%2FeQM%2FKoZvLAScvjcW0mkFX8GiTtMe6iHtdMGl5OKmdECmh%2FRJCeOQ0F5dFryHrnrh%2BcB8Nqf1hFd0jwSCz0CdDjAYOUCJ5IpEhvUoa85Gna7v%2BGw8XkV4vG%2FmK14oeVDR%2B%2B8ieMVOUTs7b5WFGJ7zkg5MT5Dm3eIaLf1GfBi3ojwlUcioN8X3Rb%2BekqVCbL4q%2FDhnLGDKICT6OzK4Rat5%2FaBlXLvdgDj%2BOWWpYOOMKGa1CKpY3uhl2BCzgnWBiKTCGQhPZ49lWn4LdhdOl%2FMPmnFWAuV0T4plELpnliE48bYd5G%2BYBpp%2B3iV7ZMz0U%2F7RNcio4tC7kxwQPJ0RpSZpfDXvePP4MG60oN7iyEPyHXFfnFWfLpDeXnH6yf6Omuw1MJXvQG%2BN6mRh1udOH57DfbDo%2FYi2oDmaRga1l6wmwnZxEBqJT%2BSUXbqFAiJ0JZl27v8FJS8Ec1PtHbb5OvGIoGaz1bSmKA%2Bgy9tVjgkwhvjKxwY6pgGKhl%2BBI0vDR5HaOVSdS40ScIt%2BtcHJ%2FhiHz0a4RzBWFTnZPlv%2B6R3yKiep0TlJVSGntfNstSScPR2KafzhTELZJakuJRW1VYy4VUBbOEJXXvKNYsms%2BFmlnocxpSKMQu4ylVcD8mow0UF8bLsYJ9wsal4mRF4i2Uhl4ngdFKtGatzBRUsnSAjvr3FHGePAH%2BEqsVdmuJv10wo%2F1sGuovAKf5WXNd8v&X-Amz-Signature=962826df06eb80ae1f44f31922b41eaed9735b75eff109b77a00e9583e21db1e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

