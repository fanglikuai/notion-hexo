---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665B3IXJSZ%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T150053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJFMEMCH1lvxEaUkD3qri6zbfywMFH7WbabStkMC%2FhY9is%2FW4kCIGW5ZbFl8Qv17%2BYHrw8GhmQQIRuMoPldfEDAHTRIhD80KogECL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwuKTJ7xiGJYC8HhQgq3APG%2Bv9YYwQKZlgd2ABMUG%2BrxSjTJs8vvnkdipHU3DqkmTSvBQXv1YPdaKaq2uxNkF4rfx7D9jUM%2Fdz35NUUJwVwOh%2FvfN%2Bv3C%2FUWgNOEZwF8LElVtdGjZygWxJG%2BTGF3dkFyRyIT5pY3uUyy4glnsZafu3YYULvns0OVESnEicG4%2FQpZx5X8dWEVzbK5SD1tSXQ2LrrG43R0mf5vtz6bBHA1bqxydHDQhJ42AMZlAGQx1A1gTM8qr0G2jnTLIm5pPULDOBeiXV9twdT1Uy3%2Fgyjj9yv4K%2B3Vhdgm2%2BWoJXWrploFFvF2GoK8Q1KUqAi0t0hU5klMSbKJbEZ%2BYBGi69%2B0bv2wnjjHcmNcV0hgOIdc3M3GliqDvysq6P56Tt692jnpN9M0bZt5OkHWSAdC2G8g5t7DhFnpOZPCPdwefCYGWt0qOknA6mzYOGxqNJ%2Fphaeq7o9z1MCOx4wpnNGamd8hichGij%2BDuyCrJIZWz3p2iLH38%2BdW4IYO4n%2FVj4E9Ldeo4P8yox8Cqo8T2ndf6bGtCYqfiC16Orm11z3z8yPYXwZZxn3yTJgSO1nC4LG9lxBjcHSebn%2FcV8QfYGCRISlsA%2F2B2nogdg0dsKZmUdBk281jgUPAr56tv6RSjDt3a%2FGBjqnAd4t%2BQndTwFLz3DQ3c%2FX6GHRHp49G2yzV26AictYbLghcNhh%2FQ27Cwl0KlpwNMMsgJkYwGdKGzHRr6UUqRtQomZ86Q3w4U8wjD8uLTUiNgtYgqZ206Gq99FHAfnfRv2Z5f%2FyKpvGTEp9gIfP0rQt1UAYL42JWyt6eorauywo1Nm4%2BhbnyfDPMwq8%2FDLVHxtInxOrcOrqprU2cLMesWYuwuD6yMyLoG3A&X-Amz-Signature=cbf68d2c91e47fc4013959719ddf29c9625029ef4bb0962610b6884666daf4b4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

