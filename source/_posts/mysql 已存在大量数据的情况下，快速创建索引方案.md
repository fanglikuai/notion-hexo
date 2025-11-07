---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UXWZBDOX%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T010037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCiL8b0DsNG1%2BWrwIEWg7AL8eR1B4XIElLDxBtn7M9TbwIhAMYXj6%2BHAurGVEDbv6kSKozoYAEC7xeWTbypT00ybrYtKogECLL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igxv23GSBHpiWvSATUcq3AOVLcGRPl5RcoT0fgjRwoiFzWso9MJtFgOdIvMMX%2B%2Bsicas5viMSDvM7614FqqXr8WTEuBvkVlQOl3EQ2WTioFwxOsejjMJ6g9Z9IzqMM%2B%2FDQCIHQaM6OtsrpQ%2FCE6QzvXhZvmOf%2BhxyijUdJ40NbwZX2uJEm2lF2Xyg0FjKy%2FX31WbLPUP29KlOnUJLj0tIF6kPHF8%2BFqFTpWk4BSIObMtTEG1O0Ln2Hzy6889XKuesW%2FbPs7RtWM9JE3xYv04NnH4475tSTYBZ7tdoGfV0Dwxg2fXoNCDXjHsNH9k%2B79toG1kDZy%2BIhquQhExl0WX6lZovVx9asB2NY%2BFAtrkt%2Bsx3957GI1L4mKjdqI7GutKBVxGskqu0GnVtCTeC3LlirWY42JflFHEQQKewbQ8fhqO%2B1nLOB48q6mOUxw4zu9djCwi132YGxPlIHgCtBuOY8ayVShmpT1urRwbPyj9FjFEtCQjmpdMWsAIbitTSSGNWLDsJJHhOhUOGJoufmHERHDZL6%2BOCjfN1NLuCN43eQAfwQpOhFx7spGVZgKBBhykUngfeAiEQqf422LfMRhk1zUGl8PMag2UTdBLhesGB0DhoKKUhbDAW9%2FQ5HY6YoKk%2Bq3dVbN4AAAJ2rw12DDPgbXIBjqkAUGOaB%2F35QOzzY3S0q6cDEpPEdXkYiTPSzQbqj8oo4jMvvPMkWDCJMtU7xVG6j3D716ZFgSCzh6ju2s016ceBo4Jwy0N1bMcnYW9%2FZ5w%2Fy0vYDf8jzLM9U4zSTP9MUN9TpznxBJlh1FW5Y7GIz1SQiq3u5WhyqfV5o7so%2BFv6Flb0p1WY6dtK2WsQVY%2Blc276O%2B9fLzW3NVRx60c3MXjSOBkkdz%2F&X-Amz-Signature=f1ee1f7d1f068e17d6fb20d0a501bfb19b1e3ce58f3a99beb58cba67afba8962&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

