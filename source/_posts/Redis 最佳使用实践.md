---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QKXUEZOS%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T230043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCSuBuAq%2B1y4uoksjUsXT0UHPI00XLExRB13k1Hfa6R%2FQIgZpmmH4hIUIF4SVuGbrV4jEpBeEWmUnlIdKpz1TzC%2FTwq%2FwMIHxAAGgw2Mzc0MjMxODM4MDUiDGvbTTYYcwOQGtjodircA4Ps3CgFNJTiTc5O4mDhLD7TwKzJcfdrYf3x6gMKwsLci7dEp9riLWSFd78jAu6AkR8ZsaV9ok2XQt7W5Mo4A5%2Fh3PTlfe9oEpN0eHr7ySl0r4OjVy%2F30g1My9%2Fw7hKSHw1eg3NDx8sU5B21Ih7ihSxq4YjXvihR%2FiIjDSDKb5%2B7WOhNdv0LhZ3INpsNWppkXTXC0WQNiMYcO18lVxrbviUcSaJPXkOLzcZuJNDYOmyFLXgVl2yDvLUU1FNQ4A9HulqbVFhS7NhjrpUUgzW6PpD%2Bpd7%2Ff5uiccf%2F0qJkiKLP6aqd3aB%2BCTa2N03tVF0i6kd0%2BRMFjRYnOQ%2Bd1bJwd2R9n2DF6oPxSIJKVVq7RwcqXcLJyhFfkuTpDvlV%2Fw7iFWMVeTttI8E30bGRqTk%2B%2FMqF2Z5Ahw5lU35luS4Mdw3lwTkKFqrrswF5qngyuF8vS5Wab0mB9YwVCEpn5Pi0oFm1JlJsNRN%2F7%2BFTNUVgPBhjUkpKbUVUryylg4%2FOl%2FpuoZ9zUOiQgt6wQ%2BRVs5Ld0nfLt5S6z4Fm5xS4%2FZ47HAUwbzJP2wxTznjjimJipijYKoRdYouguEPYIZTuUgndckRwJguDpxeeKFrgRFw08BjsP%2BwYGDeEBQ8zhYrKMI3gwcYGOqUBgimtMW1v7eos0SfP8%2BAA6mzW4qvihKWghtEZd4nzkj3WKX%2BW%2B8kXvBJOIixyChH%2BgUoBDObhkzku5G6cn%2BrbLkJByRUUckjwcYiAsPkloGn8ZhFu1HBmojx5qzLZCqwyji%2FFLGAU4tKI5ZJDUf9342rywFg8BYC0mnUnkg8ZJZppWSLVMBQwHFD9OYlqcVP1fwh4ShGqXfQm3r1d5o5dckqqx8wy&X-Amz-Signature=69a58b72f5a8334dfd05c8fe112f8fc667a3cf73ec7a022056438617f622ddf8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

