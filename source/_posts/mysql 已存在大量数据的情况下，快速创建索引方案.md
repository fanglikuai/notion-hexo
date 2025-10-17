---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663J4FDKHB%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T180056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAIaCXVzLXdlc3QtMiJGMEQCIGj%2F3w9t%2F5nMieRMOILcj7c%2Fs5UjUFYmS18AB1%2FuiwI1AiA44xyBkeBkX0IaRBMeuVHZC6qM21HYbydCrk%2FI32gLXiqIBAir%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2FLOc%2BYsiUTURiUoBKtwD5j1OuE5dtwBSZZKOtwkTS8nbQCQIP9AJNxA7WsTqtxgKva7n2QbFwHv8oZkq0gYPZWf19LhMfKDuL69qvS9WWbXP80bcaK30feFRlc%2FT9djg7OJyYmWR%2FYFUqSA7qs7rQXS%2FEvcVwOePqFk9Sk6qBBsXmYWq2E00O9B1njCAQrvvc%2BA2npsHFsJgvc8V3lTWUzJO1bG8qsCDuxHaqcYOGdTmJMny9mgqAEakpPIaWAXG3Jkng9FjBfBRd1Tt1TmkagXEKcxNZG8JymrEE0HI04a3tzGRwnIR8kwBjQCs%2BNinUNPXlT1xFhoGv8LFurAjgW6dDS0d7e5c9Qvxu2n1e1qIPhnuD%2BLrBwb4K%2FsqzBCkzKj2%2BbH%2B%2FUMoKOPGGtjErO5huVvpScsRurDIpMNpLlzm9C8%2Bxng37siAmBdVg1OouYWl8M24vwe6B1z3wWWRxAI%2Bjw32JHtZKFmUglAhNcGzXnZoGBPtaPbBHysASTZ1vSROtarJ3h63JQF7HcUv3QvJdhozYgywqmV%2FWnPkgIji9XYh%2F1%2BIT%2BjAsK2Qqb90Fai4XoPERSQoKVYeHHt0lsws5iwWcaciYPytwgehMkaPxWfC6bVLlU6vaMkLtciKYDFCd05TYu8s%2B48wyfzJxwY6pgEXcbSHy56bxLCpbrCxhLtES5bxlx6VWawDMdS5X6nMtK9NHQS9kVxE2j4rbdfM7iUIWVaW5%2BGjrWQ%2FIhAXdyg48HD3XR75LC8smY%2FLFJj3haYB7G0jJ6WzHLpFeQaC53QwaR9V22p9f68ysp6Y5xfHz0ZiqFnwh5koHjStGbuTGNGPAxFrt5ykSKTBkX7o1t59mnOONef5l197WpHH8Yx8dmQ2gQGW&X-Amz-Signature=880d82031b750cf3f401affe32212ec9942f797d04152a8577b006b31e45f515&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

