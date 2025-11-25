---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662A2PJABS%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T180046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC3xE9fXcRHM7AKjT7m4MdYiGBcdGlixq9FsCdTpuvF%2FwIgBj%2BZ59UqtQ8MYANSaKlwXp%2B7XIXClM%2Fu2HTfp0t6Vk0q%2FwMIcxAAGgw2Mzc0MjMxODM4MDUiDEtwnwJ%2FK4RGuSYNHyrcA1lCXQPuptKlS79EZqDoHjod0X04ktwGnRDsFq8pftXj4BuyrzXY9%2BHdXoW5pGaKktzRJCTHvDZtw4szX5izaRDktqUUNIiAAQr1DBeWZU76ue09UtERVSVEhW6b%2FafPbowCLEyh2x9qJMNMHoX0udiIlehafS6VAb5jOG5CfDmEhIvuFH2bN%2FpKbCerbady3GcGZOQ%2FYZErEJvFRTZ4UoreCdpXP672VZ50gWmhbE923xYIGNKWppvvXJ6bZvrl0TNx9KPVAi%2FvvQ0Zmv%2BBGf9TiU6Wzgm3o%2Fcv0IACAsFaIzEWH6vETZ4z6sKDDa2zzr4LPNiCiGWt7rhXHE%2F6ajFL2%2BJMugGZA6Qh9zDcyIlzsfw82heL4pASQKfAdw9gRXdXVqZrvRRGXZ85lPZ2XfqhpRIPFBsOMuVCexxTJ3hv1Pw4ZLe4cTEmeTIjF32tP0thLsPBXpng%2FOZ0WmJgmhrWpW1fYmO83Qgr8KAkxwpfYadYQEt4UTLovAfniANh7Rme%2FQCLGgwUadG5N1R5mvwzwQ4jpWb9nwKNrdh0FJK4BMSsmM88ZAKeMp1X5YnTEgJN%2Bl2%2BwjEpsol2Acqc4GrAFDMTyz22hyUMJVb5HWDKZpvSt%2BKlycqe9wkdMKXWl8kGOqUBlTZzGVa8tFPA3Q6rX1eWimTbwD8gLAFeqxbS2TeufNvO9lZIipVtlWkXiHQXI8quxPr2sHCjiSKXYYO7s1T1CSiGKhEUkulYBdP%2F33kMCaMT1XasMQCMkce%2FwJpdAvhb1e9WzQDl88B1STNwZCCOZTbO435E%2BM0z8V8jVtA5yzR7dE2hcjaDepXWCJQtBSH8UHlEayyENWbtH1MIGdTQ6dWGuPey&X-Amz-Signature=b4b1c9122c4ec95e2d3f5ab495b41992b1e6db8959869fe6531e864c862f8102&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

