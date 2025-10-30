---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VSBYPRZN%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T020041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECoaCXVzLXdlc3QtMiJIMEYCIQCUEeCvJPxhY4bNVtgZM03FMI8LyOR4h2gvxF6Kx102kgIhAJjutMYMK1mSlmbiTvZBb6U4eOC0Ecs%2F8xSosoikcTwRKogECOP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxU5yu5wwM7fUcEKhwq3ANseY0XsJim6pi5IK%2FNijVutH1s0UhoS7aMc%2BfLt58iAp9ErMEp4NqpTs5ecUYSsoghCCvxw2QvA8iUeRDkvXSSQ3RBcgxsshMatNI2OYbN4q4z8%2BTOBQaYY7yfyaaUxKnHrEL7tVKdLLIjH1d%2BdMHHLPSWfRr1HYoRkH7aJjI1au8Ak7nPAIKBzbf0t0vtbOwVsIdyVrkNqqisoiIvKXPm0QAe%2B1WCC1DxIsbmS2jT22p%2F5gOAkJQr4AFQZ8P2CVcNmw8UCyKzkvZwie2SJMwT8xbM0wmmtZraoUjqlyn0XILX4nOL0lehV8oFh10xBga2JS0qTS7FuljqszH1BdmV9SFbAVET42tDG5Z2rfVjWW8EZVlYOf9ESovJKdyiR8oKvdzUGMfBY%2F%2BJuXZdiuXETemXBloQsGmNszOkeuCxm7zwj3VHtulA97lgd%2B9fZpEASvw68cUsQW5T5DIh05HwKLI4Ln3n4hnYTtXqjzZZKLAd8CFy7GcchT1HalS4aNZ0BULG2dstgBv5T7XHG2sG45tj7Ai1UAIlHFb3SGTydscaziZ%2FdvFt3oVrU%2FCfS0g8F1tQGtCS%2FlsUtK3x5MgE6SBDoBQBRkbrJXuvIhQjf6hzkzFy3dNISgyj2DCXgYvIBjqkAaBB8AziR52yeYvXjKXd3yp8ECbcPMHkokZdqHztuLabPFHgn0E6n4HtckgRh0e1YihvnTmI8rCObovs3KWTxpGG%2Bg%2FYLZMbD5xcrjVyRYZi2Ww1gQXotu1jacykhwwtdvwVMImdOlIiEgo80JNj3R9ufQCEdsD73hR5YIIkY%2FXY3ojuyD8uAgNmPqDLEhW1VpAPX6duS9I1b8T3u%2Bfn6E6IPYlM&X-Amz-Signature=86e5fda05cf448f6a3e483ea51040b257c828ac1aed02dee6c13254afe3f736b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

