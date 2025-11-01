---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ROXWW4ZL%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T100047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF8aCXVzLXdlc3QtMiJHMEUCIQCg54wyZkn62eOKr8LQr4LtQknFo28tBDLxKibj%2FQoY6wIgfKBWrqJCeCD6k3llKGjmPaG1PN%2Fl%2BpVYTG8T9b9z2Dkq%2FwMIJxAAGgw2Mzc0MjMxODM4MDUiDBOyxsY9qUpTFFYSnircA%2Fq%2BkfnWhwJf%2BStu27XdbVyNznN2udm7hPFiMY6xUwmD1V5jy5zDExro86OQg9e%2BLRLNNm440OdIAuvbZmuIr5%2FY0bbMv1BUBPQJLjLeyrTcQWNFC5bKHYdPeAAnRM34V3TAJVXIL4k3YGq9%2FVW%2BW4oRnlk5nT%2F9tBxm46SCVST%2B86TDyV0p1rDxkMyRooC0%2F9Hct2AweheKMb7Bk%2FJ91T%2B3M54aP4yM2x%2F6LOsfTs%2FXhexDW0unYPblcNX3TUWcH4pR2fONiP7conMMAwLkp5DyscprIMuS2%2FQrpMeBZMv%2B8gZZv7k4nCafa2z6z8U4d2O4lzJeujCVn7VzktOo2LHrcNa0cxzHplWTZQu2F6Dh9FSXeKZ4TrV1ynUntFEEMBjYttx0rBb33o6zBN2xnVhHOTQw%2Fd9IBnOEqYVcNXyAgfCNwXJlALhJ0SKDnAAG6smQAiPSlYY5BOZuvhfnL5%2BHKbscTL0oLW3A50tuNlEH9PAZbdVOtaRqbJYKOd4q3lunHa7L20SBDc0LoHGVQE%2F8qoOOe899lRTC6xz64Aavn9rNzUYy%2Fr7Tg9JxmLBielizqWYqnGevG7X2DxROEXHMKUowsC8cnHpSn9YY%2FDdBhW4W7zjOEqHH%2F4fgMIDQlsgGOqUB6FM8b8BCPgLe%2BtnLPNTbiyNG3jk1esn9ms58ukP1LIrh%2BzDkdcPbSwaUlUPpvUcldDXlnlbWwE1KZiOMAEidGUxSFqJ89hPkNu3HtXb5oMIURocCGUzuSJEFHCS0BsiH2HQCVMEU8iHXXQprV0vCqfiYRDmcmH%2BpvKUfPgnhwl6obg8WRfIHREmVsjOvXIYrmM2twF4j5d6gF7bZd0MMMb7Lv2mL&X-Amz-Signature=fed7d529d003ea0a23bb8d1caf54a3e2c89fcfd8af67d5591f345119d38562c5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

