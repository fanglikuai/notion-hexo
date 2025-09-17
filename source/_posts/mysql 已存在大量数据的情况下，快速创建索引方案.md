---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZTJA3NCI%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T150045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC4aCXVzLXdlc3QtMiJHMEUCIQCVn6JNTgZIvEsCWlL%2FgnfMKo4fPMm3y9qtK35XZc9K8gIgNZvTpVzpNygTCdWo%2FpujB7Nkup0hP7ADRq0OlfCN7GkqiAQIp%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJQOlr6UQEvC5ZCbnCrcA6KOj920eG5tyX91TMA3TZuZ5k%2BogiWZtWh45evXPcBVCEKUrR%2F33Rto11P%2BFUZKBXhZG6pwUpaZ%2BPLNIMNSKj4abhZ5iYp4wMuOqOW02BCcA0uODnBugILZgdJ%2FHi3f3z9pCbs7X70fKgcHcLOK%2F5FKqHfJb1wiA4blaAChIR%2BJ%2BngvsuHAJpafwBjUOTy63lU66%2FizoOKtK4EWvCdgtOZ0ywkT%2BG3w5DHckuX8SN1PT0uyLiSNSNnKOV%2B7i%2FyLhtizJM1rJscsWJaY0CF8gT6cdEFgE12PEMsCWUnK2PyeQmgmG8XvrcYF5E0J8x5dh54NZoUKlLOWauwMsGMzeS3xXYyN%2B0NINMJYJruPC%2B01jsWBGydTMlAYhX6DyvrG13b%2Bvq8WIs0yiO%2B7YxsLY%2BQJPpTjNAEvtjw91FXkhZowmRBDC2pGMTj3ThWz%2FivuSTJd21dIOR875%2B4dZ02biLcWF5YYVENS9iinQdLU0mHQcSJ2dzfLAMuMmoz4dQwQ3mAjxK%2FhVOOqamNBVC7jwTBzgx1S5hsiGj%2FWJwxaYYEHX6FkhkY0MR1bV4jVUlgnK0%2FXeE2HW1gdhttIDLhbnhp9z0xGZnl2tTKfO7SPnYP0ii5bxaK9o5AYQAt1MKv4qsYGOqUBRLSfRbsx9q4fm6M26DSWDuNRsGt2tL5VbgthlksXH%2BunZUu5YHAwxHAmlHgjokgxM1R%2BTH5ehC7su87HXDM74%2ByT9tfzaDQARZvck9hKzgpkwD%2Bg74aW9sH%2BbN7xH1i4uTjyttNNHq8qed4m9fU%2FYhPrKCoxujlRCyCrB4%2BNUxUqTsOi2A%2FzjvCEGX9TOTc6oB2Mx19xKnVYvKMmyUOAhOLEQJzv&X-Amz-Signature=f42c4b2f5260385dd1d4a759dca33484302e8aa16a6dcacd69d383e045de121d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

