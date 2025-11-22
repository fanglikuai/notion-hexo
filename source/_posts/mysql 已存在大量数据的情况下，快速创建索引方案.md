---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S2HOQBTT%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T160045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJHMEUCIQCoVKojA0Oe9DAFX42X%2FS9uRs1A0QsjcyZ5kO43b9PPZgIgVC11VNitWbRi4QTZ03tyitghxRJJ0ZyxlKvSh8FDc3gq%2FwMIJBAAGgw2Mzc0MjMxODM4MDUiDATBwefinMClWa5r%2FyrcA%2FAyVGOyr7VI38JjLdP8J%2BeaNkjhPPjHgsVE0TjrOPvGoIDRaTVRt6KSRFwajkqjcsL7Y7qBwSnQYgo%2Bsp3OLM0DlLjYKzbPU6BQyyDBrDi3E8LzLl2tLb1gJrClh2o4bAvMIMTq0R10UtZlNr0rP0KOEVFVHPkW3TlTtoDz5NMZZ%2BrvbyPoynXLtVR8nXeTQTYGp5KnlO5Uyt98bz0oAwi5lI8LFxsKMopfP10hIjf7tN0ZSaHhbFnBCZgC1QNYTZBA4WyrL%2Bm0GyLvhZZRqSt3pv%2FWRhG7a8zpBYbv9ELjF1Xn0zIcumHpD8oG0RqnUSWOxv4RKS%2FBn2lx0xjJr%2FbbJD5QbMNbApddUlzzKp3Dy5D0avujXqjRbb3NsJyRXepQaIWd8OtdIrzpXziR2T9YYmQpPD8QIFJZqb85cu9ntPFMo9RoR5LShxu6gulzuq84bVmrdEo9h7mhXiVhlkBa9N3l5lcVitybNoM%2BQQpVpxyD9DNb6c1WA54gw9ssTbVgq0TRSeqpBav%2BGCTD32mDTIVov9K%2B2DFlKpYnULTuWIux4HaIsbH3hYlPyB81VVDc11dHVTuvzjaevnalYgJfFJ6efK829VG41ZVQvcR%2FPbGkJTSTgh%2Bdk%2BsIMP6ghskGOqUBbNmI8Ud%2F9nIK7ohMKaKZgOSzV84J4ARNEztyYj7U8GAxpMcG9C0LJWnSuZ8UXCbBRvPHdKmR71o54RM7D%2FydCQruvUap4TpQ5dlhujcraui%2F5NYQ6qwHb1IfhZsQtkanI5uNdC1tTEJTy9A8eTnZn8lPhmPB1ZjW8M0EM9UnC8QbA7RHPfoYUK%2BaPwyylz3ocDUIbSvnIj4KdGSNkQt%2B6RSaWoPi&X-Amz-Signature=61dd35437bb8c827f912f76092b53d6149c1174a409e54f45827bb49dfd1af54&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

