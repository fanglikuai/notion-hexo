---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RYDQEPZJ%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T090049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAfrDUH6B2vQZ6hay7feNn5b2x%2Byt3TwSdJ%2BQdiQ4OnmAiAmeZNelP2HaoNXMFCHKyuT3mC%2Beoqt3EzGUxjaafMCqCr%2FAwhZEAAaDDYzNzQyMzE4MzgwNSIMJP2yLvyd9R3wSwKxKtwDT85%2Bv6ESD9tg%2BM5fFI6QFOvJodY8%2B2Fhn1SLCC%2Ba4Kfak8MwNiT8kHFwIqxyvoWdJUZOEayhP6E2j49bf2lywzi6XATwFbH6Us9sbuYuCdJ3inuTfcWmO1mW7WqG6vqbOUwfINmJ8dde4%2BG3IBkVDq%2ByH6Ul5g%2Bag420pM5GeYYFVTVFpY9RBS%2FeaI4FgDJDGL1hDHXLwozvK3hOJLSyjH0%2FIlRctn3R3cdmUb9RxWgQKi71NoCvBs8fk4L2hqXgFXoGKsmcIAle4DHmNnNYco5lHi9Gv7moiRgyw%2FIZ2nhXdOX6DasOoW2b4hNyP9opTFh5QT%2FGxQTMc43Lqle7IdVZpem4UfECQDk575rJPPQEVK9gP2sljGi%2F2y20JVEADPPl2PfSRpjjbxuFBYeQ1nS2cMH7LeaTnkyQiAvmEUDL%2B8MdghQbzaHJkXyarW0aA02jJ81HOpl1nOw3igg8DhNvFbPwu%2F4Yt5PaLrFYDa3NJPSfavOYCLh1DJrlmQgv7FXaAt%2F9e0nnHZOVunFqbJ%2BaOnDx3hFYdEz8UrWUWRDP3Au0J40CZYKbQ8KqlVpTF%2FQkg%2Bvh0dhOOEMRQsq%2Bp%2BPwm6F9NzyKPAQrBGZGVA1pcgZP%2FAMmdv47CDswj9POxgY6pgHdelQ%2BJh8VVFo%2F46BXMhEooL5pXvoNJWMt59hzNsVAJUOsozYMdrBcWrIFczrZwtUVrPX8k5iPYR6%2Fjl2VovmHZ%2F9AmF0M%2FmgJoaBfbEvBvdQuq45cRLFIxEfC%2FYZbfrFh18GnvbQDOcD2qaoq7SD074poajvfsfTkUmp0%2BNhdKr6hXt81jS1nbqCAw8HmJPJkgLY2fn2HEoVDpx7gQJqK%2BYXBrxsd&X-Amz-Signature=edc62ab7e7cd9d0939a093e525d1881de345ade0bf0e4318abe81305e57dae7a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

