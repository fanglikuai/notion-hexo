---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VS6FYRIT%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T190038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBsaCXVzLXdlc3QtMiJHMEUCIAJB4wJvtTsAeHyYxi7Pnh8fDohWhdgWhKRd%2F102Qm3vAiEAobn268fdtY%2F3v%2Btvn0u%2FubBa5k%2FkQU%2B5UVSpJ1Fq8scqiAQI5P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAU9%2F%2BoaHwGm9TLubircA5F0gFFQ0Em8zxgi0Vu%2BMCk6n0i5ufuHtjmKfbAY0%2Bu2dGN6XzQ8KFMLoxIxzz89HyzUbS2jsKeOa0Q3tbvMIIiFb8wrUdFIjp3alwE0803kU6LJGu1ckqe5Hql4CKLmVYht62pvbIxmV%2BKGpojW2L5krl1ciRuBR1Cywa%2BSgMyZhJxHjd39I6HrBSEuU6wh4w9ZOvlp7f%2FFXE9E%2FgMdcCh7wrk63sVbMQpMicYRII7opUrVfMEeSbHhE551opyuakfNNmu72KXTCR6c6RFyMPJk%2BWf9y3fS7QdVwfjMnVGT9beNSN1xmsbKa8pHCsyVMAPFMNZNM1gfSb8SnXCdqW0vpVtnHC6uEPZsIvcRPxXIVI1cSq9PZuNo5%2FpQiUTUOhJ3h1kDQSh4NjCdYaxrcsOkxmtMsPARX6qcb26gIIiaEXI5ij4Z8kLM86lbToq7H9moiPZxQ5fCZNGDe7rCvh4xryn4Af6jgjUBfs7WwBard35oIW1HkhrUhf0zZ%2Bm2Wva%2FLwi8nXS6skPmSHSoakSeXEXH42tzBIDy7VAqFyyE9XdKmodMEE6gPkP0g4tSbOj0vDp9fSXLwh3%2Fs19nvJVMzPTURvE7E2BgUbxP6ayXoxQDtLmmMCIgfQ%2F8MMue%2BMgGOqUBwWHuVEXKzyYHXwyZAV%2BKxWoHuFYTWBSPW%2FGwa1JFM5kFG7XA5a1GGCowcSHR%2FTKZRUEDVBgLSmE3gjig4JdY%2BDXxStA956rO%2BwT1RpKgm%2BGFcm7who8tWAGoRlp%2FaaFcg4si6kKMjHllzW2Ybh%2FeQ85dVPPTOEizTKd%2BO5jeGwHy39VN5G8Ng9lgihEKvqPW9406rDy9f6%2FgNbjNHTdzgKkhpXYf&X-Amz-Signature=a1bdc6663bb1241de14a89fa9f85c6062827f27bacafd9f2de179b94db18baca&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

