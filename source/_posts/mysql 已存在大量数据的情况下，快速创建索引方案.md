---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664BG5F2A2%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T180045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECIaCXVzLXdlc3QtMiJGMEQCICfv4O4irpvDWi57qgsQArffDAgeD2w8SFYDSVuu%2BsN4AiAQjWGCw7ww1SjeuHnDobL7gR0%2B4m4ShzBCf3p67SoHryqIBAir%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMTDE813ZHW%2B8hrxuaKtwD%2Bhj28kieJSP2jN%2BcZqmoahdy4zEsK13npUW3pHKK8MHByHqkk96156h5brX6aIN3rRDsqY7CletG4MnnsuR%2BATkLPV3r90y8Jghzh3I1it2nxdXr1jpUujpOsjg4HtRv2E2EPn9KVPtmV1zLNNlM1I03hq2Gf1bLHNUoief6QCrszlv1dTUeE7zv8HnPC8Gqonrhig%2BqpJq4oXX2z5qL9dNIOj98YnrU07bFeTpscU%2BlFN%2FREKSZ8MrnXAN2E1RzO%2BhqDOV0c7uIa%2BpFHMTkx0uZUl%2Bt5pbq3iwF9E%2Bp0Hr54yryO4Go2gZY%2FQpjWg%2FWA0%2FZYWstyVE6lcotIQS%2FWjBJtKC%2FkXftHnX5QbYcK0Vqf7T1FSZIvJgRQkRNoq7NjP7bP4wKZTuFZ9gHMN%2FY8kqH5vUJosSzUf8MLGUMfTnX84wIkSo8y%2FBnRqNSSK4GUWT%2BbW3a6%2FrekZOTlNgz6v9Js0AzEcnNrLlHEyb5W1A1CltUKfBykA1PMV7o676yZ%2FgFK32X%2BPRmSP5olUy8Pyw7sBZ4fErgNEdGBmkcujepO2oo2h0Qg5FjDQ%2BSFzqbADtVj2izM%2F%2B%2BZxE5ju9i16jvDjposamZDfDrLx6iL6caMQIwclo6su01jgww88jgxgY6pgH04B5tAr60n6p53I%2F8fN%2BxoIIhhcG3nmEq4NtXsGACz15Wazkca3b88Ccbrcce4Ubbpcv7ByQcF%2F5giImSxUakIG0WoGu6FqrShxGsqweM2EOf8nf%2BZpc5Rm5erjYRaaH1IXECN0gGnd8i%2FWigSpIT86MO1w0BXmFIPFKGNLjdTHi6%2BE1O6ttn7fCtSO2b%2FkAtqyqSloTIsPU2H12RzaHzRg9K6tuv&X-Amz-Signature=65ada02df7075bd14f06c8622a3b7442aa4d18768cf5d91f2d384baf6558b895&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

