---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQZOVCAX%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T050053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJGMEQCIDxkSOaoGPWetFSBnk3GvPYR57P9tGf9uLC%2BAx%2BSEobjAiBLY9siAODTvtcYUZYrNduPfMnWfCk%2BcF0Dg51x4ClOXyr%2FAwgeEAAaDDYzNzQyMzE4MzgwNSIMQESNSsrZR7i3eje%2FKtwDSoyVsKnPsTc51IzwtM7yzq3jCz1TP%2FO16A0RC71OzDPisfQ1rIEk7RCwOlBXsmyL2NBDAfpzt74JjKwbOhxWFqjzCRAWZq7jd0U7uS8kd7%2B4WjwaNp8qHS%2BCj0LSLUaZHAEFxYZg2GhaV6GiLQMxfMkVcaZVFzclLqK9qiMVETyjZkQkEeWcCyWHykRguNFRnFt41aYWjrDNYgufpAlk1cV1uKxr3WcIy2A90WDv2mVdM6ALVCfEDEiaG4X%2BA%2BNmTD2ZvubSy12uSSHHWiLR7NEs0G1MJkakRXWGBbSlLxmKABz7%2F9YKOfYhWIjrLAKtq%2BVeepsDTRrJVnKtTyYRm1wmlYsvMqe55D%2F1xh2OzSxR7YHysA8tk0o27KjK5OUc%2FUiKvWfEoLQKx%2FTA3wZzVDRczu7i3r4HrkZ3pJy1vJxkJ9eq7HJai3ukAY1eJ6DbbcI8IWp9SoGOYyaA2VPk4RrA71kR0qDfp7McfPuSEmD12uR6gsetmmigkv5p%2BGblPjkJlto%2Fl3mEXACgawOaf44%2FPUctQP2XC2iRUclPoC8iKznf2HnTU98j0QAYOjzc0kqTcknGzRfwZ%2BCQ1K9j3WAq%2BM%2FbkSZmrYt3DwRBiTmG2dmN2HwTmTI7Yg4w34OFyQY6pgHS%2BCmEDO6sIp0sixz2fG4ifsjyWwQ9bYuvDeMBVo8diKbaBm952UvGuqtmHwx5beW%2F6IswlgmNamjqbYTuF7dDN3aI6iZp%2FdCzhfohxRX8zQaZREOCg%2Bfl3YcwEXL4zLjkChE5I3tzPbmgVo7IjCYjYX%2FQfvXFRnwz69M%2F9e9Q7ss6tffyGPbNM5HUvo8f1Be9et1WofXHWt5jwNQA1Br4W%2B89g2%2Fd&X-Amz-Signature=adf770fd1a6bf42c0f6e580cbacc3ccdc9e2492be83e60666104b353cfa9446d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

