---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RPZQHXSX%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T020041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCIQDbmyMJRRhkSGr2eCy%2B1ytgBF%2Bd5dUK86lQzxEBWdzELQIgf7sj19VPtPQkfGXP3abwJyqGRH2n60IPSg%2F%2BjAPhaGQqiAQI%2Bf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLK%2BBycQEqORIhsPLyrcA0b5GmOIFTX5iYCs1VCUNZFALbVoV2QeF6YXkLbpGeXHWR8LcxwaLmfRY2ohvHSBuxTlA2kqNmKjcRMyr4%2FRUeS008D7YGKJQfXJ9YabfyMgLJsuHCZGa0mq10%2BtwwyStH0hjhA%2Fohu8XK6KmHCV5aLMT3XM%2F8SddrkOaIgTFPYZWsiuSzDvpcyIRxh%2FSoIuLaIKwM5NdeDZ6f%2BN3k9UtOuhoAgTEzcrajH45h3ZpsO0vI9mtezI6lhWtwcIRTzZq%2BjQPLsXwE01E33s3ErPHV4Z9kDextxyrGKM2srM6zRpTg7xSENsrtJm84wwFRhuGYcfmEvSBqk2IlKBfPJX7b63O475FmwiYqi68ETz3R%2BIqlncGEInk%2BDqeMcqtAjtHABZa9D%2BwtfVifB9JdV2dQu6rq0OgyNoXm86VBsrRKxLwRazNJbQq3VV4PG5h%2FtAb0nJwQ8Aa5mirgLtanLi%2FHmwDydO5m5WuUXlks%2B8wMOcrbbw4Ftxb0EN5nAAMaNgET24QhtO3B%2FRfhSjnS7hy7eGPco%2BskHJQ41KRh5e2WrNwLCyursZX6a3y2WsqnO7OFbeYvcCPdBY6doP8jRlo64zOg06gn%2BPpRzaOPo6%2FVeAZMO6aUgWY%2FOsej5MMNXq8cYGOqUB0jip%2B62j4YmLRZYszSZxTBFAjyC6SUPy9iHfdvGUFHXc%2BfOJOxAm5tLRaqe3PfSx9MYaRtd%2Fo9ZRAUPo4fOi%2Fw8NVJHB3hZuit%2Fz3V4ZKRlc8PGH0%2B96ctrsFSHDAYZNg99ezHG6D0LKe31hymz%2Bo1vjcMO2nU55vc7VdAeZuNP8MTJMsaKFJBPCSYlCmisKp2P%2BflnpdfGSCtT%2FJLM1m4D6EYqD&X-Amz-Signature=c22b90ffbe2824db75c7a700b423e7454755f77c513bff2e3491fae80865ef33&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

