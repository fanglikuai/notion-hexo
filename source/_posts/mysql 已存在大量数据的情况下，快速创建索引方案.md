---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QGY6ZBN6%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T110040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDjZlzSqkbUv32MUiCoqR7qaJSgyRE2HocOI8QnWDZjtgIhAL61dfyeySxA7XR3w5HJ8VZhLulsgKN79JWh6FQGQyc0KogECJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igzaw8C584DAduqLCNIq3ANLkuHhL4jIJ05%2FkAaYsRMAZU4DpPiZChUilZ0NmshrHKJwN5maPc%2BNVpXOqrb3tCAztdRnbkeoCvMxYRQknyssAOAHdZdZTaXX9%2FxrrnD8mpuJYHYyIuWRRO733X1wXFzt65u3QF9jWZ1JPLF2EOsxHPqordFvmgMS3QToWNqCrRpduEeFREfqSBWVwLSSTIRffajdgVSSbdQBjEt%2B6JKPIL3NBYt9X9rfbHnOmCt19zu7pVSppvSqmfD%2FRi3oJSOJy2xtbwrD2Jj1b2KH9JohSJoVOVDSgm9JydpwWgvtn%2Fzbb291tSGwltYTVxx879vwEzqofTtaY8IhKksbKaxPBoPUE9xubtEXSyUwaIX392C3h3%2FRh1aTNJfW01xnOhU9U%2FlLrFPWANGiiAtmVAlOrwStp56i0cgZigf6IGMjU8o%2FIc6FZ0vrfxwu%2BNmBXXAGv0vy03QXjEhqfiXWViwY3Xgv5xhpaJLqsnhlTLKCWvsoMy0L%2FGzOy4w0Pja9s6u%2BNReY%2BcP%2Fk7cQrXOkB7l2uHpidv7aMfTrcNRvyAKHhYNsmpIiMDpGwFynzk0D9NBGt%2FLJ5882iMHD1nag3ZQDLRgz9st54lCgpZtxCVx1g%2BMRqJBrTaiDhVLoWTDNpKDJBjqkATTYJX0qx9B6t%2Bpds4%2B%2BVuFxJEfpXsreQKuvErPgWz%2FyVop4CpvmcsiTy2JtW1QS6OEbA0%2BWTdqN6ICYfl2ZUYZrP9uR59dOD0DnCyWfWRTOwjXHyAou1lkSCfqwOLrR2yh%2BBsAMV1cGu3CPsHlb1%2FN6t3HsgZ0iqDJgBtDS2xhjU6hUy4ceyVeCdMVHsDhJTei7gQIpkh42JDGqkqA7bcZ302fX&X-Amz-Signature=287a0f6b062e09259ac4300f1465419d3ad1bfee17451191e5833d711e2937aa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

