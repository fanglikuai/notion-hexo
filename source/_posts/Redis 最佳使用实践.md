---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U36DDQW6%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T190045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEgaCXVzLXdlc3QtMiJHMEUCIH9pxv%2FVqRkKWNL7%2B7CjUBNsLAdnILHePfo4gsKJuK9pAiEAoTpZpRG4SNw4h9cDQiw5aThfRS6%2FnJ7ICm0JeOzvK2UqiAQI8f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNfQQhTp4YhzNfNwaircA8BPtPggnuM8o%2FNgQgMYKmJd8zE1%2BH49%2FOy%2Fv%2BF65YyWP29bEoK5wDxnSCwYP3scln29uWU4RByrimX5vDoUB2Hc3puUThXYHEPthcC8CspSmBx7uK0bZAobCCIV3j0ZgBm5C1FdXzjovqZ%2BIoow2FJpf%2FO40eZrEa84zzvLvElS89OwOsaANounzPbLRTNQwHiBSsri0Ze3E3bRRv9TQgArSX5wEK%2FXVR6XiXrNNdGDh3Upk4XWhIdYNCwDsmjVmPdaopUHKAfJaT0O64QEoZQ24I5kvlNpdyR2SpBDz0MtZlgD6iityyu0ArXublZpSM87gMOq9ETuK1NiyO1iKFO69jY5PSBT2OsZi5rfVlNwTDZrAsT1uTK%2BjHgwcyjd8qFWIuDLbQY1K%2Fd4kGS8VS9UP5FYZlSyuhJvjS5xrkIsLK1RtAMyQkxn1HBM0cUYb32hFB4ujpcGbA3L1uVD6AbK1OqggHb2aZUQ9U5Uv2kNORjE04DqNRMSQXzdXdEoM9hBQ%2FpJF7eaPWJ7rVqjSgCi4nvNW9Ob38cxmAoSu5ysP7G1a4sQqTs%2BSNB5oGwjCAcALMFTUcGdTMewyYfLRF5I56VKWX9YdX8o6VH%2FZMyFYtsur86FtwlEkMKoMMC22ccGOqUB%2FVWNmUuJ4NyuRqgLw%2FtiugYif6eVG%2BYv53MNxnRbOrofkeYDIVgef0lyjHdDL7GiHPnQ7bJTVBA2QbKn7iQEZ1QqluzyXSv51dYyhv6GCvr1ojHVIGPukOWMw%2BL%2B71a%2FdQMhzxD2oQBVUXY35Nk46lrqLJX5DdI5TZmGRq4jOqbAEFeQjKalz%2BENoGFT4scJYvarq1n4WoIfyuNvKlGzaiOzcIPJ&X-Amz-Signature=cfaeaa9a8164695a39f7e2cda20b2caaa592ee1e85af55f686dd79f085a1f93a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

