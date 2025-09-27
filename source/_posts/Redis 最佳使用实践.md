---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664BG5F2A2%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T180045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECIaCXVzLXdlc3QtMiJGMEQCICfv4O4irpvDWi57qgsQArffDAgeD2w8SFYDSVuu%2BsN4AiAQjWGCw7ww1SjeuHnDobL7gR0%2B4m4ShzBCf3p67SoHryqIBAir%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMTDE813ZHW%2B8hrxuaKtwD%2Bhj28kieJSP2jN%2BcZqmoahdy4zEsK13npUW3pHKK8MHByHqkk96156h5brX6aIN3rRDsqY7CletG4MnnsuR%2BATkLPV3r90y8Jghzh3I1it2nxdXr1jpUujpOsjg4HtRv2E2EPn9KVPtmV1zLNNlM1I03hq2Gf1bLHNUoief6QCrszlv1dTUeE7zv8HnPC8Gqonrhig%2BqpJq4oXX2z5qL9dNIOj98YnrU07bFeTpscU%2BlFN%2FREKSZ8MrnXAN2E1RzO%2BhqDOV0c7uIa%2BpFHMTkx0uZUl%2Bt5pbq3iwF9E%2Bp0Hr54yryO4Go2gZY%2FQpjWg%2FWA0%2FZYWstyVE6lcotIQS%2FWjBJtKC%2FkXftHnX5QbYcK0Vqf7T1FSZIvJgRQkRNoq7NjP7bP4wKZTuFZ9gHMN%2FY8kqH5vUJosSzUf8MLGUMfTnX84wIkSo8y%2FBnRqNSSK4GUWT%2BbW3a6%2FrekZOTlNgz6v9Js0AzEcnNrLlHEyb5W1A1CltUKfBykA1PMV7o676yZ%2FgFK32X%2BPRmSP5olUy8Pyw7sBZ4fErgNEdGBmkcujepO2oo2h0Qg5FjDQ%2BSFzqbADtVj2izM%2F%2B%2BZxE5ju9i16jvDjposamZDfDrLx6iL6caMQIwclo6su01jgww88jgxgY6pgH04B5tAr60n6p53I%2F8fN%2BxoIIhhcG3nmEq4NtXsGACz15Wazkca3b88Ccbrcce4Ubbpcv7ByQcF%2F5giImSxUakIG0WoGu6FqrShxGsqweM2EOf8nf%2BZpc5Rm5erjYRaaH1IXECN0gGnd8i%2FWigSpIT86MO1w0BXmFIPFKGNLjdTHi6%2BE1O6ttn7fCtSO2b%2FkAtqyqSloTIsPU2H12RzaHzRg9K6tuv&X-Amz-Signature=7ea54a945da548661544e5070f5adaf87c049099d0ba60e54564e92b2954f2ea&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 23:53:00'
index_img: /images/4f27264a7e8afe769a5c2813552aa0f8.png
banner_img: /images/4f27264a7e8afe769a5c2813552aa0f8.png
---

# bigkey 问题


![1753077336565-23eda3f0-dd0d-4865-9f4e-b536a19e7c9b.png](/images/c6758344cbe13f3ebf0f8718f40ab3f3.png)

- 使用离线库：将 Redis 所有数据导入 MySQL 然后进行查询
- redis-bigkey 命令`redis-cli -h 10.66.64.84 -p 10229 -a xxxx --bigkeys`
- rdb 文件扫描
- 生成 rdb，转成 csv 进行分析

删除：


底层介绍：

1. redis4以上，默认使用unlink命令
2. redis4以下，string直接del，其他类型如hash分批删除子元素，最后删除key

# 大key进行拆分


采用经典算法“分治法”，将大而化小。针对String和集合类型的Key，可以采用如下方式：

- String类型的大Key：可以尝试将对象分拆成几个Key-Value， 使用MGET或者多个GET组成的pipeline获取值，分拆单次操作的压力，对于集群来说可以将操作压力平摊到多个分片上，降低对单个分片的影响。
- 集合类型的大Key，并且需要整存整取要在设计上严格禁止这种场景的出现，如无法拆分，有效的方法是将该大Key从JIMDB去除，单独放到其他存储介质上。
- 集合类型的大Key，每次只需操作部分元素：将集合类型中的元素分拆。以Hash类型为例，可以在客户端定义一个分拆Key的数量N，每次对HGET和HSET操作的field计算哈希值并取模N，确定该field落在哪个Key上。

### 缺点


本质就是取模，需要在客户端进行操作，限定取模的数量，不够灵活

