---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V4T47E2Y%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T110041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDHTRdVyRUP8RL%2BziDHj05dcM68qUR8UUEaYlzczI0baAIgOABzNWJrLb%2Bc6J3WJ8TI6ya%2BpsWiCB1owN%2F12f2vVygq%2FwMIdBAAGgw2Mzc0MjMxODM4MDUiDDsUtCJE6HtUcjfAfCrcA%2FPlOHLAg071sjzqOPazZBPVtdoCeb7ywsQQrXRod5pzTD98SJWioPbd8GxrQTkvEFTjdNZHuTTEndBpEI2pa0cUHqIVaS7dMr%2B0MSd0cE9Qygl1h0HgyM%2BCKCGRXR00n%2F0hS8tiuP2viQuhtYn24fODR0SlxyPRKUX%2FNvRhm1l59epjaYrGyTUj%2Byo1Va%2BDj2d7fPMtwGG1w5y36TvVZT8yaOmvZDQQzVGtKY4JI2n3ShNgwfqG83Xk56MQKdJUKJOCfwjwckXvKcZw8wWMUaz4MXrClfSYPHljZ5f7JcirdoHFOH02XmOLpCNIDylcd5GB3Bu189uRQhUpkHouCTcck1fQ0DS1WIgxD9q0B2TZ2g%2B9I1zQf9n2IffZzzjMBIc%2BKPI4kYf0RRu6ugjGJRBSsqFbas32WVu39O%2BcL%2Bt7hABA6wNM9ixC3QZNQEv599SN981UAkPhsS7F7l%2FAlzseYZRZ3EKukGnYiWu%2BAddxIaIBpJU7vhbeBHllXgwVaHx8azIIUTOVaa4db5ZMrY6fP1HLYwVGKdThyWCbmv3764H96paY2zfpoJHhCDV%2FQyv5L%2F5mZNJPHVnIchJzEJL%2FIwgjkctgjE%2Fd1qpFI078Mc8HvCElJtVKCLX7MLq91MYGOqUBSeNyg%2BsUTGhHWjAp6OfJtW0%2BZIaFIftwD1RRRMvDCVqcMzXq2iwPHIQ5kR%2Fy%2FioDhdWOO%2B68ht5wj5EO01OH6wJXlDM7eKRRJpsIIAoUHt9oxaF%2FhBhnniOe4iqFLZ2NytktlyIYOCpOkhATi9F9DFwXw635bwijK5zac0bUrLz1gfz8q64L%2FHj4CPdEkRrUK7A5iQK3ozF7MQV7R0QJ%2FTYteYR3&X-Amz-Signature=d1700616c6aa02d8f1638c2598d04a4b4c0dfd817e2c3b2007b93470e8464289&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

