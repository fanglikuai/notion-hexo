---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QGY6ZBN6%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T110040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDjZlzSqkbUv32MUiCoqR7qaJSgyRE2HocOI8QnWDZjtgIhAL61dfyeySxA7XR3w5HJ8VZhLulsgKN79JWh6FQGQyc0KogECJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igzaw8C584DAduqLCNIq3ANLkuHhL4jIJ05%2FkAaYsRMAZU4DpPiZChUilZ0NmshrHKJwN5maPc%2BNVpXOqrb3tCAztdRnbkeoCvMxYRQknyssAOAHdZdZTaXX9%2FxrrnD8mpuJYHYyIuWRRO733X1wXFzt65u3QF9jWZ1JPLF2EOsxHPqordFvmgMS3QToWNqCrRpduEeFREfqSBWVwLSSTIRffajdgVSSbdQBjEt%2B6JKPIL3NBYt9X9rfbHnOmCt19zu7pVSppvSqmfD%2FRi3oJSOJy2xtbwrD2Jj1b2KH9JohSJoVOVDSgm9JydpwWgvtn%2Fzbb291tSGwltYTVxx879vwEzqofTtaY8IhKksbKaxPBoPUE9xubtEXSyUwaIX392C3h3%2FRh1aTNJfW01xnOhU9U%2FlLrFPWANGiiAtmVAlOrwStp56i0cgZigf6IGMjU8o%2FIc6FZ0vrfxwu%2BNmBXXAGv0vy03QXjEhqfiXWViwY3Xgv5xhpaJLqsnhlTLKCWvsoMy0L%2FGzOy4w0Pja9s6u%2BNReY%2BcP%2Fk7cQrXOkB7l2uHpidv7aMfTrcNRvyAKHhYNsmpIiMDpGwFynzk0D9NBGt%2FLJ5882iMHD1nag3ZQDLRgz9st54lCgpZtxCVx1g%2BMRqJBrTaiDhVLoWTDNpKDJBjqkATTYJX0qx9B6t%2Bpds4%2B%2BVuFxJEfpXsreQKuvErPgWz%2FyVop4CpvmcsiTy2JtW1QS6OEbA0%2BWTdqN6ICYfl2ZUYZrP9uR59dOD0DnCyWfWRTOwjXHyAou1lkSCfqwOLrR2yh%2BBsAMV1cGu3CPsHlb1%2FN6t3HsgZ0iqDJgBtDS2xhjU6hUy4ceyVeCdMVHsDhJTei7gQIpkh42JDGqkqA7bcZ302fX&X-Amz-Signature=34d4567caac6829246cf1d18e8bcc22531c91b2d302da4e57e4b0efd1735d31c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

