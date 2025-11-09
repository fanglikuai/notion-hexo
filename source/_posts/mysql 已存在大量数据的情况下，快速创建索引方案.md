---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V6QPC5U2%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T020041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBgaCXVzLXdlc3QtMiJHMEUCIEMee20%2BkUOAjnF2ZX2wqstMQkFXx5B3f0INWu0NYZm7AiEAlNQdSAYQTtlA6W0AKZrSMjkeJypt2cZNlwHsxAIw7DIqiAQI4f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDh716Qm2NJOhrvXZSrcA9DpQ8vxuZliumuzzFDknKeq%2Bv2Ul%2BeWr0dcKyatLJIWxBBwoJdYGzCUIxrW1t9f7BpL6Fs55KgaBuGV%2BcDKUJPvpbjS9Jl4eSWgOvFg%2F8%2FwL5xW8KGYv%2FK5dAbTtUR0k2CEreVY%2B1J8liGhGG3lnPA3HhnGYd5XCApN9R3OT7AgSYUQD1HgEyq75%2BUcpPw7%2Bb%2BgRTriMXH5C8%2BZBk9p063PzH914l6qFpgghmpPlD07W6%2BIUL8rMpRNnJeyygpbOhjEkI%2BQxgSaJmaApXC0lrviglhetmXzx2npMJ1248MkNVAhtg%2FMM03ZHr5u4VKLR4FxvVNaXsLpFJMDxi7tWliM37G%2BxpNMfFVhCq9MF4%2BC21bKbinGBqc9tjhnwQv67RCisZ3qrSQNKKiB3bclHtvkEdZ39KR6SYDdUtcPTgbQtz1nDBx0m%2Fl3%2B0UkYriGNuD8X6pZsO1yqaOGs8MVpsztUxRZIHBOSrhOD075Vwo1W%2Bouy%2FL7HDa5ZT08N2NMRHQAlvLwD6erL217kgldbFapBWluUxcIeWD95DX8uC%2BT4a%2Bc%2BAMAOm%2F8ua7YLaM7jRondfoQD765zwYrWdDNAca7BuGNXZVlPJO7lTSOJhhQEqFZPZwmznt87Jx5MNmmv8gGOqUB9rTIWn404T1CiZLJzbDoGD2tUF0KZEDjO6hK%2BwRaf%2BeXDKxyLbVe2%2BmuDRXkZT%2B%2B4POUsT%2Fo9KnRJW0e2HGbeQbXI7xyiBH8VhfFWT0MC1spK37z4tYRxKPK3XWpvCkpGypIQGQVgHVSWN%2FaXlSW0E%2Bt60htcIs5pplTalZCFMwdrTrousFLBfQebYe9zJgNoZWZOxO8QE1p5jC57gUEi5FpDXUB&X-Amz-Signature=797a9bfb762e19614dddba580808166e3a8a0cacca470bd576e18e8a05697401&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

