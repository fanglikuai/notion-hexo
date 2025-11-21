---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46662IMYL2T%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T170043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEkaCXVzLXdlc3QtMiJIMEYCIQCudVeQMEXBTwnivbZVonRKs5YO2EfqAlaI4cf9ATJPUwIhAMi8%2Fdt3eO6%2Bqy0VyDQnm39cIX57n7qRR0X%2FsdVwHKPBKv8DCBIQABoMNjM3NDIzMTgzODA1IgxmlBs8rieNMFcSVmUq3AO8dXbmc220AFgTUmdc5GykY1MtCrjo1p0Q0FEtOnJ22HkXiiBt9MQGrV%2FFt9%2BHrRCuVEaUw7af0tYOrgNlOsT%2Famc4fe8pq6%2FKr0n2yA0kJAoYN7%2BMiNFbd99umLag9bIkbYc%2FxHBNqSm%2FxjVf846s8JsX4j%2BRaozBK%2BWgsxl2gViMNVNGlKqnnsCPSM9FlwVXzejYe%2BnrlYBzWJB%2FY8jkyyUOOKpgUtw8Z9T%2BMs2Mkj8F7f%2FRUtyK52TKTGhgTwWEzr427CnZsgs6k6jFODHjMMmPpKQdJYJA%2BmCiS%2B7mvPXpe9epLrR2ihIMMzimBA9eJZA8eWInUfpUen9tgwimw2jFbwllXj9HYFpeKaXCGamCvDL8nZTYhfAY5%2FUcow7hXukpo93SIxMvosjPLwCU9UN35a8qthyQKKnwuTeJepHWJgyeqgI23OagXFj3qkEpaydRj%2BaahE7J8N%2FZpF7w95WJ3Gm0HapvmAtzjtYqBN6i12WhpttpnXuDzED%2Ft8FtYuDUo5hY%2BKjcmS%2FGs%2BjJX%2FpC6Q%2Fu3xOW%2Fy0pIFcG9yoQCMXC5GaIzRCzzK6td9d1NhKUAIszEpmEMP3xNnWq8yv1npcZXAfnpfqqrGGPF5qJxZkfbh%2BWDo8WGTCtrILJBjqkAeNPIBb1tGKZD8H23%2B%2BQtcRGAibBSnMVaQXjbmHnFQ2KJRpxtktpXSSm2M7ewf7oXzOFphGtmhd6dtng%2BOQakVI074WIyJDwCPHVh3bgeAOfm8TDsbV2j36UiSezqGmYPq%2FNd0bZFSo0QnBw5xNv9qT6KyVprhIgi53d01ko%2FuZf9UEpGyfOF3MdJ%2Fp0mraUPSopZ7OYIwfI%2FlZeR2tG%2FwXWXyJ0&X-Amz-Signature=7a0e00bd3559b36a676f58ea5d370574e2e77d1471d420b00f89c588af62a406&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

