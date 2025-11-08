---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z2V7VSSI%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T040212Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAQaCXVzLXdlc3QtMiJGMEQCICJLiI7BMkbk9zQaDVHkexC74GlR2CxOA2B6j450H0ABAiA1yRrw3gA3ZpK9Jc1DYWf2E5TSQqswmqN6xuEOT2DljyqIBAjN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMv9JrUjymcu8lABx%2FKtwDmJIeZix%2BYKPTs3H%2BQ25vYLXqxVbSXlEnM%2F2t0ld6fjAfr4KKPMK4J7uXGAcG%2BXHEcCLkvtpE7%2FwUGkImaft1ykGEzOVSNStbyf%2Bkzt4ZZzszlKSm9gT8trE8iPFw1ZltXtNHJH1geVvcxMQJtBgTDB7aOinQuFvL6D4L%2F%2BDis0npQkYkPUeDJP%2BmPkpWvFxlVnIb%2FfRO6K2%2F3c8%2Fl46VTeORRIZb2%2FoFsGjVJq96a5dzbtexHuVJqaMwKd89ZjMkimDKKOcH1fZTLsDVXvw3WuwBT4Y0EIEPRiUcvKIpz%2BLhDa2rpVvTX%2Fki%2FOVjNG6IQAbW5rb8rW1apTYTf51lm1rinodfNBeqMbnmmtgQUrEn0O3uAcv4cV2P4apWizhwHUoF%2BM1Totqe5tqpiKKAFk49s19pxxu7UeRic3hfEAhtprlUdEMXVxxJaPl%2F3m8xE3uhv%2Fbp0xE5JYY5B5ki%2F0mS15Gd%2BsbQNBQMqyxEoyF04knaOLVbeMZBn00AQw53oHjPVitEp5vYqXkCTtKBWQB6XnSr6pG51wZzwkvxcAlnCSE5rDmt82mKXjXCzL6XQvXo2UUZkU88q9eXYaqUbJKD%2B2rxr81zBcr3NBZJx4EoQuptsLazq3Qd2xwwifS6yAY6pgEHCF%2F4QoMXuYzP2EOp%2BBgpmTRwH7AOHvYL%2Bgo3ki19LgiMdv2sP4Pw72f8Yj7LNp7tW2u4RjsSYfOf4DQTwxNnY1%2B%2FAHqERIUeZ5yc0IFOh%2BUlugApQJsPy%2B5C%2Fuj8UiULrETdGgWNZR%2FvjSDjylzqwPULEuafSaajGkPJdi68QY2hu6DP%2BegSjS2bknkuJ211r8sThSmqTp2Ey1NWJ8Dxff6Z%2BIGJ&X-Amz-Signature=c8297afc37eae44a33a2c47e6f7bf6f446ee17e1b8946daacf8b411b7caa6cd6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

