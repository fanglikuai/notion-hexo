---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666STS3PTY%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T190051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAIaCXVzLXdlc3QtMiJGMEQCICoSp7U9vOS4zu%2FK46mrF2gERzZuEIhzAIbe4F%2FlINMKAiB4a8JKpes7AyENF3AYmC1td5aJjZiA4BdDBBbrXzLsOiqIBAir%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMzDV%2FruiSf1sq1CNTKtwDyXoMrT4xz0yNthHsZ1wZDfUPiPCmDAgCKGYKBmi1HcB1TtHBaAOTnCmW%2FF9zlFAneMVFz2C7TKz0%2BAH9wp%2F8QPGtXyNtNgYG41q3KyWTBB3d68ulGS43LRE3Iqp9Ks02YRTNzIfvNXMSkG8zGq8%2FDjxOXg3yMdrvdB89XD5hWRt4isws9Fv47BxjzXXGsAclANcjZcMWf7fpVlIGsKGG5PLJRcU1Jyda%2FMu3HJInz8TTiLRuQQ%2BxGy%2FixtDb%2FmBK6wXWbTarw4WDRzSxas7SE8v1DUGvB5hB7dU7cqRoP%2B0H0XnW0%2BddaQLtu3nB4ORTQR7wQRgJcW4OuVg5ce9evOWI3FknwfYf1%2Bt3hEGHEypt2XUBLRW%2BYH9ChCJstj6%2Fc2JusBVtVapI7BQjPtIkRzCLeFWFNbgw5iR%2FXcaHlz3FLj9nd9Ny2YJPhpG8hhokuasMLMJ4vDExoUF%2FcolE%2BdkcN1pA4BmTBrAFlpi611hWOW8tkz9mFj9SqdNpnwRAfPkbGHicK38mdNk7b5fCjM0Ro55DWtYNnOokzmoDINYkKny7%2FicrYarM8kLLPN%2FQMDcfhr2TG4K5sg6nD2TMl1te%2Bfw52%2BG2HtaVDfHYTlrqrQl%2FaXm%2Bl%2F%2B%2Fg5Ewuf3JxwY6pgGOfkz4MD6BNl5%2FBuedgOdDf0D9YmwDB3T7wNM6%2BDFMnIcFWK1i7M1FkLHFkol%2B1j4T54f9SU8Yr7gvbVT%2BgNgY243HaP7WkrjezjiyQrh2VSQmsBoOfeIdnMzmQYGyy4xMmvfdhTJ2hOwtzrVqnL7RS2yft%2BK5tjUcOs5KbGrwUUEyAWSu40op9JLfY31fdjhkGrhvsk8xVbQollLEe2HlbMldb8uE&X-Amz-Signature=83964ac381c3d489d367879145da7fbaae4d7c9c09fbae7f014e9fe7081dd0a2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

