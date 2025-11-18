---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YBDADZVV%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T090046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIE2PVEvxDPlpMMLc47NKjlYVpHMaiZeAexLsrht3rykJAiBB%2FtrDxnO6LhSFE97UClTSHEqgAdZj3MP%2BpFL5L6T6qCqIBAjC%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMEE1nBQE7hx8mfPyqKtwDHxPCnEGUt1XbH%2FVxwiG7CWQepxDYqWw7VZZzvvEDkv80xL7m2U5PJRzV%2F09DrsPjZidJUWLRo00SAuRv8HFIgfZu%2FZtGeIofIwMtuK5AtA5ugVn%2BvQ7BK60O8VS%2FN4JYN8jfQTQIojuI0CazHlivdhGj2umBcuKhO%2FwkU0kIqSPN7CnanVU8qp6SXT0UUEjX4Ckv7EeScwktRxu4AWFKf6xWvvHXgjsv7A1Rz9mrEQ%2BqG0GMTY7fP783jNq45aafqy%2FrXIQRtAVyqVHLhIbwD3L5WsBJM9xWtOYqMj%2Foa%2BFK0kL4Y%2BESc6XI08E9QF5bMWZpwyOChayuQz3IuxUgUYUJMJCgKYWEEItUal7LJfc6a95U%2BGtuTuW1ApUl4F3pclDeELxHskLqw%2BafcSUYmp10omzRCcPkGBPLZt26noTCuxKnghgF1u%2FrDNK5p%2BcH1lczaTcTUtTprurnOlYr2%2BUafhD%2BHnS5fFhidjdt%2FCX%2BA%2FxSw7X5Hs0wlPsp%2Fgd14l%2B9VeRR%2Ba0BLCHMTa6ug4u%2BOF2XiU9Gigvxr%2FRvAck7srASoSmrb7u1hDEb2nOlT250iP4V1qXZXedLPDk5ECfpQeQeHrwK2CUIUqcVOuImm24WuDwfU1NUNhYw0eXwyAY6pgGEoaV8aWs0NQzp8uwDYZWpXuD%2Ba%2BzRKoW8LoCLcjUeIoNLwrctOq%2B1ZEoqePnEn8gnENWaFEsdMQ1AE1OUeXTJxI6G%2FEqgRU9QavpXDRMM%2FPr%2Bi7CJPs1W6hxuzzj37r7acWU8eZWKZ8xHJcQaIcuV2ozg%2Fz54eei4CJBM9KU2BVmjeHzGmLl%2FSHx4%2Ff5yTgZ5iSVV95el%2BOCQgR7C6eQluXRyV%2Fbo&X-Amz-Signature=d07e2fc5b80555c614e4abe157711e48ca104cfcb13d7b0f4ec6b81fb71a0778&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

