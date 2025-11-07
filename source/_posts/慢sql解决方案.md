---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UXWZBDOX%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T010038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCiL8b0DsNG1%2BWrwIEWg7AL8eR1B4XIElLDxBtn7M9TbwIhAMYXj6%2BHAurGVEDbv6kSKozoYAEC7xeWTbypT00ybrYtKogECLL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igxv23GSBHpiWvSATUcq3AOVLcGRPl5RcoT0fgjRwoiFzWso9MJtFgOdIvMMX%2B%2Bsicas5viMSDvM7614FqqXr8WTEuBvkVlQOl3EQ2WTioFwxOsejjMJ6g9Z9IzqMM%2B%2FDQCIHQaM6OtsrpQ%2FCE6QzvXhZvmOf%2BhxyijUdJ40NbwZX2uJEm2lF2Xyg0FjKy%2FX31WbLPUP29KlOnUJLj0tIF6kPHF8%2BFqFTpWk4BSIObMtTEG1O0Ln2Hzy6889XKuesW%2FbPs7RtWM9JE3xYv04NnH4475tSTYBZ7tdoGfV0Dwxg2fXoNCDXjHsNH9k%2B79toG1kDZy%2BIhquQhExl0WX6lZovVx9asB2NY%2BFAtrkt%2Bsx3957GI1L4mKjdqI7GutKBVxGskqu0GnVtCTeC3LlirWY42JflFHEQQKewbQ8fhqO%2B1nLOB48q6mOUxw4zu9djCwi132YGxPlIHgCtBuOY8ayVShmpT1urRwbPyj9FjFEtCQjmpdMWsAIbitTSSGNWLDsJJHhOhUOGJoufmHERHDZL6%2BOCjfN1NLuCN43eQAfwQpOhFx7spGVZgKBBhykUngfeAiEQqf422LfMRhk1zUGl8PMag2UTdBLhesGB0DhoKKUhbDAW9%2FQ5HY6YoKk%2Bq3dVbN4AAAJ2rw12DDPgbXIBjqkAUGOaB%2F35QOzzY3S0q6cDEpPEdXkYiTPSzQbqj8oo4jMvvPMkWDCJMtU7xVG6j3D716ZFgSCzh6ju2s016ceBo4Jwy0N1bMcnYW9%2FZ5w%2Fy0vYDf8jzLM9U4zSTP9MUN9TpznxBJlh1FW5Y7GIz1SQiq3u5WhyqfV5o7so%2BFv6Flb0p1WY6dtK2WsQVY%2Blc276O%2B9fLzW3NVRx60c3MXjSOBkkdz%2F&X-Amz-Signature=61dc0ffdf677e6445e4420f82807ea3b9081f3481bc9e4dccec3b9a71c16b507&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

