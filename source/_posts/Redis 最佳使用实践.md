---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZGXFZNEJ%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T040100Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFMaCXVzLXdlc3QtMiJIMEYCIQCfVtxRp%2FrbhVodCwFSLgL%2FBn%2FvaT20%2FwLk1CNWEQV%2F4QIhAMi8FKVjpKumiPE9SFknwPrIPFlXwv0Pb3ybW5D18GyNKogECPz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxD8YXlSFDJ%2BwSK94gq3AMvgVOalU4qwtKL488EoZez3X%2F%2F2d5zqMn35ndOlHcMsJFothTr%2FZRHNJQZKCN4AdboMeq8VwA6yjB1GZKfPitdnLdUlIh1G4oquvmiKuVy4h%2Bg1DOkeyJUod2FshbtLUM3Zkzdrrvob7TWnWzFrN%2FNt05HRL%2B7eg1KCiCsDL37QUeLBmwiTWNHYR4GnU7x2aDERbviKp%2FUrSfnYvLhv39DSL3IjdL7Zop%2Fe1tBY6I2aowyzNTxwz9MwRLID%2BbGGI1fHketbsXfBb0T8d7XekSzUqgVHxwVgyxsXsGmCyd0HI%2FqhlgBMeXIY4075QKx1Sv%2Fru8SalALvyEB6cmrT5JY4ybYjP54clyxRHN0%2BwfSB1%2Fkz2yx0Hsaj7LS2h4j6ZrovE4GqhPVnmP9j0Y28%2Fl0Vo1qIRtBoVU3jSTClHtj%2BPaUTF5ShfXUvLmhtQSKsHUVXqSSmMEjVCWjKBjkfYU5ixIBO9gvCsaqfyGbaCwz15Ec6rEF8PwnVpt2U9CYk4%2Bpx3FeQZ0fHzHiYTo%2FA0DkRWdTM8nPomMrsUl8P14PC%2BSsP9KPak8nDo%2BvZMUykQ6rmmiLEIFj7PlEG1OyWNqL8OgDJn22FyZIfnTsYBmJQefuAveWaqqVWdZjcTCd6tvHBjqkAV725nr1GbxiMeWxl9kZSZ23xFU6ylBFF8X244P76yieKaTS%2B46qhPNOHOFjLQZ1zkgxWWDXu3yzSlNtkLYlDQdyGMsrsy7d0c5kvIqZIZ2H1u%2BAcff4tlFYMNZD4nb5yHNMJohGdn6y3DYMHWEahelcKFjsoUn8J25l6q1UGVNaVRPklrbz%2BvkJHF0dJzLi3O8JXFUFfJpsKUAigK3WofOzwg5z&X-Amz-Signature=d2b70222db4ac65b930269198e3647b991f34c95415fe65c2b84b9b3abbb3085&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

