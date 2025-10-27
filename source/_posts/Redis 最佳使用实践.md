---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466URP2V7W4%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T010047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIC28MriqqGkpa6fIoHqPFjbylvHSqFNVx9%2BBEXjXonGYAiATE7tK0qhzdi0jePP1IFovxvIC3aKLVRkZScazz%2BmJfCqIBAiZ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMdsEP7a%2FutS64TAF3KtwDvdBgycfqwaexCLdhJDLhnJmlfKEkWJquMnals43dhukhtZDbjkmNvi4dGjFkzW4uRzPvF7SoEjZy%2F%2Bin%2FaN0glnaZmW5lBU2j1GLMLmfcbLkH1CWu5z%2F65HdkiMXUCZtqJk3b2rjq%2Ftw%2FmrrZX0Ni3jwYIpjRr%2B4x4uPY45WaFcvFhUexRYYZA6heQN1OBN6T8G35oL%2FkCg7lhGgCjhnq84Ln1phhNZyRjiycx3kqeOJevpS4ZLUxirQpIBsPnf9MTBBZC3oK23%2BpvcF1ov43eSEZI0LFk0fWU%2B5o8BHRmkMtcaLMBTM48EzCpqDr1ywVas0fpN099V61dtXMx4ZVh6ZlRq2e7ckD7iWvtS%2FdEADcsofHjAOXNMm8t3j2Y%2FoizKj1fNWqV2c7yNOMlJasGs3SeuKQpPZk9PUNOjllakckNkLF0imIYizHGag8qfw86yz4FpqO6dAgc2%2FfC7D1IwPIcrhWSgLTn%2FdIlNU0mhNUChtvA%2BcxvjfOOmMerUsQAGsFmHuzb9uduaMDs9h0%2BZN5EwTjj%2FiDuO1NU3EfKDre1TOpzPYtSydz8O%2B3u3CcJ3maXNZFMhl%2FXPBItsozbhhTa%2Bc0cARrTqymtK8Uwk5GK99i9IxHXFeqiUws%2Ff6xwY6pgFawQYAksOQEdIXXgkGmo%2B36Y52pR9qs7u90BZqMK6KK58l%2B9OAsZ2un1hDfNn6vJIFml8emTZOJrTvRaZGFmvOWgllakurYyIL1XfwH2EGmcvT2KZ0%2F5Vt8KmYlVDQbLb6f%2B%2BJ1cy89AY%2FV5g6uwlWXXPauvdOMq4v%2F1FG4DXJXIJEJ2mnv5klswayltBdq0XQdSI5tPGM7VufvFgfw8YvycgBoq73&X-Amz-Signature=0429fb9d10934069435f1cbc4ab3c6bba011ff0f6539cbb93dc8825fce461bdc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

