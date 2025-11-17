---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VFM3UT7W%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T020039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD1%2FvVs0C7yYyipem1DLej9dbuBvzT9cCHmDdvbpmMoywIhAKFQWtNEdMrH%2BpboFM99MO%2FxmaYXP0VBHtmZjnUiFJ5TKogECKL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwmMCxTK6d%2B8Cwbqgsq3AN1MV5xJxdx2JEi6VEjzSQ7Ng1usA0tHHUPQ%2B6RkSHWtgxwrmrXwpvMEbFLfgLbLV83BghyjJ2xgGstXH1PCpNN53cw6Nom9AqIqZHfZAoolorMbsUON4PtgTnf7duz5ZsedY%2FCkU10ZNbD4Q5%2Bp0kxJMUDRUL%2BLbTr9ViMCTo4PI1nYIcCdam7o7yhSI6SapgrJxZjiBHPBGPydsj9QLoq9QrJQMdzPx0e89nu%2B67m7DM4PyQXiv3FnkwpnhVTEgKDhkARD5S5K6mKmuTIkLKivt3gx8Z%2FTO29dXmagsVdXUoP2XhjDtNkysZHLXYv5E7p1sd9kavDFPNj2z%2BqBFyHpCkNq7IYAqqH43osYixOI0OlsamM1bzc%2BABpERAWdHNgcdKrGJ934KjjM6h3QHZ8mRarzBHU4HelVUAev8K8dnRWtABCsKIW39D9IkxbjZp8eSRyWc7TNM1a%2FNNR7iZHywgPxpyE5Mz2ZY%2F1GlIFE9gsZvVVj%2FjUZV7%2F3u5ybfnusoe7OQsdQqZswEH6u42O2pR0iv1twrO44Z1ELkVkVzZVN7RuZ5Y64gPMCKY%2Fu0KuxLs%2BPUuCRpvhN%2F%2BnPCVLuZZnKBs5sYHgYlsG8LsqykJQTVn%2BPVyLrk%2BuszCQ7unIBjqkAepid3dT60krYzjDeBqJvQO77Wi%2BVrbRcabUs6BPlZLODV5RqaPVvqVSDfoaM%2FWm29U%2FRzkAhXJAzdH7ctzpXL3Uu0NOySjhi%2F0L34triX5w0k0mMqn86TaIrcEUwkZoG2OqWAn9s9aFIBdtFVQ1mlpV9WZOqpIwEwZXc9k7fBINl7Vl0SmOWTSE7i%2Fb2FG7aL5iWthsKpVYw4PK5mqn6XBHmlRo&X-Amz-Signature=d951af539ca3b46c4980ed9a871409c27f61a9965ad577f3b2c07829c362f033&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

