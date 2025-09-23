---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TSSW26K3%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T000048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCGC7wA5t%2FRSZNaz4ogID41E5%2BkSEAY2AiEv5Selm6ALAIgN6AMYEXB0i22L8EOViZv3cGTi%2F2EX05s0tHb%2BsV7YO8q%2FwMIOBAAGgw2Mzc0MjMxODM4MDUiDAcTSGl6ouc5ysdo7yrcA1yELaW5%2BsG4TM4yYTs9hMS0pCvpfVdgJVlOQU%2BfHstrCgZxiwGC3nINGdXhRYCTNxcsYvWnjMlH2Res6ZlfaGuw4g%2Bwye%2FDVOIdzncZ5feIN3p7cmcbvX1vor2fB5mV%2BmTDBah4tQ2uBBexns%2BXfe0DEC5UdQIOIfoZHQozv9rsKi6cKOg5efgdb2I0k4KQsgzyVdP%2BP5C%2BTY9IGTzi7NQFrUIhtVLE1V874HmtUnc7VjcXiyVQdr6TG0d249mdmVQpSW9ynBtMhFv2TCebW8XaQMs98Mf3ZU2JUpkel84CWDvM4D3xvq0rZgP%2F1nSsiv1SmCytmzC%2Fpaq4N%2BXKt4S8mdI8ljc%2FTGi4oNX7Hyrx3SkMdj3S1i2EHoJ3YGTGoWjDI%2FvLu815G6gSTWlRwzUWFzxQ92WNOXaFkTbo9mTg%2Fxpq2PIJjkLT4csgM94L34QeFSegDdXfD%2BSpNOD6Dk1uk5JBvyEQ4YUF%2F9zkuUEsLP2uycZwUj4%2B5qRbHqBUkAKUcNiKLF%2FT0gX292PUxq0E9%2FLZ7DPTQD%2BTy8U3AaZdT%2FexFXyv59aBmM%2BlukN3Ct4%2BRtDMGF%2F9vL393W9bhbPwKWU7geD%2BW8YE2Bu90hGr5XG5kVm7LYGuz2RFMN6ux8YGOqUBkosPBF%2FeFO9x92PILw5hsAWqTTqJwtfeRVPhboPoO5ff%2F4P5eWQytHAmO9WmnX1DkoApHJMGFis56WU1TH5ZrrZtN7t%2FgIxGZKPWvRd1a8RRk8462vpqYKAK7xjqA0sx6sVePVpR7XcKte%2FKVxjdPhHAnJQ3a%2BxIgJT%2BiirbQS1P8SuO1smxg8nO0uSTkxnIBg0ZzuX8qveMSpcrtCvHYSBQlqSW&X-Amz-Signature=e23dddec9569df7998955ca60e87fde84e66c7c35a6e0a0c79e42c3e38c85b1f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

