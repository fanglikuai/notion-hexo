---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SRX2IQVU%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T170045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGEaCXVzLXdlc3QtMiJIMEYCIQCAqsON28iguTAZwTI62oRJp32oWltzNz6eViKLw994rwIhALMlmlKK8qPlxqPTMhFMlmZK9Ugie2w3mHNPwUwLHGs1Kv8DCBoQABoMNjM3NDIzMTgzODA1IgzgMLgh4b0L9K%2BlpZ0q3ANlIjeu5nuq6Vl3MgD7qfHejnOopQ%2BZy0cY6eHOhY7bqLaU1%2FxbzccJ7nnW%2FVjw%2FTsNAQxC%2BdzLsK3EgH4cMiNBxn5dQwfvp5lRql40mdwTydxwz1WNX2rybsYjPImD46qLZsovDOVZPf1exr41iJ6aQqWLvzi0j4fBBw5as%2B70zUP2EYLtTSAq6hu8FRcKaqy4ti7pf4zixU4udYXyvauuqudy1Mbmvk4LzWGJ4LkVjA%2Bbjhbe6l0epAz6e00IFF4rE7PSJ2i8gunQjO7AUwbYA6R0T7j9pfUp2AmysVAYzGy2liQe2U1xBOvK4JR3faQzxWOiOGgMX6LWBRbblNjqVObUZnoh66GFwvPjGtRASvSeUSj6SOXPCy6x2BGc%2F4SOza320zTsNI4Ixx%2FTz4c%2FieVn7b2skbCZIM7%2FX8NqHpAJI%2FV2r%2BBAE9LdxTBQpwHhUXCfJ1AmpF8QohpuE73Gqu%2FsBvFObZ2yxceba8dpjUYr80N%2BpFTKT4oZMaTfstyXUbWWnrY5SOeuFS9pjJiWSC5Dp9Uv19y30oL3lMkrsjllUFM%2Bkk4x4Zq9Wm3B%2FfHnJCmjjIDbob0NdvToqMTHRLB64Bepe275HFK9CtodvYIQbAOOPMZOmHFP3TDj897HBjqkAS%2B1BspavBnClnJ%2BMq4OwXZ%2FPC5b8Nx5aNSlzoa4WoS9OGDv53QQxVEG6Cggqm5sjRwv21uROrg4hD2qd0semKJZimQeYjnNEKBOUKwAv0yw4OXcMJLABSB6xGTgBaCa8T8iH8tfdQAXlRQspVxWNJSuO%2BjcY3Oy7RIy3D62G3qxoHqoKles1uVcslGZNgjGIVnH8%2BxTaRfDB6UG7wZXdqnu46gT&X-Amz-Signature=0d8b8e63be1e0c1d46c366ca67a22d86230c15e7d54b29de46222b8fde25efed&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

