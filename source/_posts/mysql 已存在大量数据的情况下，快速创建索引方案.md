---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YRROBSQ2%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T000040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDyUryK4gDc5ARNDj3udNHucW6%2F44JG0OXxCfmTQNTU1QIhALBRWfeGonFSHtNNBpz3HaJ%2FUzHFDD4jinNyso2s4o9AKv8DCH8QABoMNjM3NDIzMTgzODA1IgyTeaYkUmHWLSg96%2Fwq3ANQ%2FTRio5O%2BETj2lFAvce8oZz1nQ%2BMbiDcQJmbwcI2y0i30Wn7MKyNW9ZxVIS9hVb2Y46G%2B2AO3WOui3G41pZPrkEqIiLsmLFwligIdRb78cD%2BR96EXlFNJIUUA6ZBPORbX76uQVPw8VfAqoU0Lsvjf2mMKYJn10GstzMOInenrQ0EH3H4lpSdjR4r46uMWpP6XifbSIvT8E%2FCZ1aQm3KZrqcsqFgkUnT7jiTxZE0VNqCAEBKRqPJGXb7wnN852QvDdsy6u0zqrP1JXQsKJBA1y6h4Gg26cM92pVjtlAu9p43%2FVY4wGXm%2B45z0WAHqSivYz0luZk76XXY0GutzlhJB59Qv9Ez%2BRJMg4sVcJZlmGwpjqcqvq%2B8BMx2fB%2FOryA%2BiIxHQSAKspNpVr6MZDqUrw3iSEDQ84uM3DbIFOo4dF9IGi%2BeNXjIAY1bszO34Y8JmCHHzsv5p3zKmLWowo8GGdNGzKAUYcz7IsPdk3cOiDTCxo6aTAFFOOVvyeWWBlOem9qY0p%2FLi4D5dUIJX6pJZZXzv4cCPSmKWKY37zx7HsuFjvPArBXjEyYvC5VqGFP8K7z4zbwcA6YyvJm%2BVA4Xov11hv3Ksq68r56nkZmXZg0qF2Sh3sJ1ocjB7ejTDk56nIBjqkAbgSOHOCilpcpGNuL9spQ1j3ca1fHenboxcHm6y1wdZxukOFrI%2FW6fDHndFxe5Tm1UhZUKHY58vrp%2Fg7KVVe0yLbAQvXb9KIKU%2FiPc3OuncmZl6Eih%2FYqUh9NKpmhqQeEE9L24Rbv%2FrhAEslQQUBTnKPzXj2zCjUl28nFaGze81QbvODjAMDx0XzM4FOBdqNtLiO3r9VDG9M4e7TAayGxl0RJjeo&X-Amz-Signature=a2225c0890b60709bac9e6f93ec74653471b31023c882aba9a6d1efcef013c68&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

