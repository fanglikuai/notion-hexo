---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YYUS3WBG%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T010044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAAaCXVzLXdlc3QtMiJGMEQCIGHviEQsEu3xdD56ZD4oF45UCP7W147uowOSXJf%2BecIpAiAIBLot3NxQzmLfbdcqszNaUimEmZUe5Tn738u61mjaiyqIBAiZ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMWEL7PkCy3eNkCWN0KtwD3LVYE0S1ZgdF8W2CmjYJQA1lRqX%2BHRx6p4C9Je4iAwaS2Y875N6aoUxcMkAVnx4i%2FxxmLdeZzsdjaqwFI6btJ7XwTOIUlRTG4QGMdchkagieAerVI9vFZ%2FLBmd%2ByD7j2G3oZPeyjEE%2BwGtbXD0HmcOuIdCq8V1hy0hTI8kBUXtqhegLzSoraBUZ81hCJvkGnKvj7FAjEzIqCbIDyM33vOh2iOsZLWE0ECu0IRi4O5upmNNL1HPNBYiL%2FuWLHjIvRQ6QoVK7BYLHB3NjE4mQ%2Fo0AflpQyuTpqwtw0Y4Oe71OGbHs6kld%2Fi6b53QZfvWmAR3fVtk86ELH09bhUE8BZfc4wkD6ZLYONCnOoM762BA50%2Fl%2FmsQ5sDVHFXNhFYnbLDp4Y774uIRF8Q%2B3Bl2sJbP4is5elLCPQzel2IY1J88%2FH6lFwzOPwIAteHiKl18rRpnKxIMgeit0tUULZwz9KLybf%2FQzGxcSi5167Nz9z%2F579glMtBiunoUTbC2LgmEoreEojzBZjQnzTLBXkXcbOg9QNf6LbWyo1DQ3YrZ8WhA5x96%2F3%2F9T4eQpEwepcLnERZcKEY0%2BXM3N7i66DvXAGXeF0ofW4qkwLJYZNTIePo%2FhBi4UKXBZWaLo5Z80w9rSRxwY6pgF4cCIMIzw5ke4fjOCQLTC5hLb%2FdEDlj4gQQ9CieO6PzzuceSKfeQqR9XvbNP2IL%2FvDL1EHCj0dLi1aQR2essSy3CA0hLgYKW168wjwYyAnwjQL%2FirnPQiwhKzxwDSvWvbURiQtBbXU7yuyRHDroXbNQGPx8axUXBlqEW8vreh6WRAqJuH6Wosr9wFM1%2Fcg8fanH%2F%2FZtNcxA1H5sllRgQnk7PsKkv3f&X-Amz-Signature=f35ab68d17bdadf7721d6beac4a9c11cfc499d5aaea321e72a511b75d2059784&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

