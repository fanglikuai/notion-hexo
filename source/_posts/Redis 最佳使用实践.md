---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466672OJIPD%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T190047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHIaCXVzLXdlc3QtMiJHMEUCIQC%2FK%2By3%2Bo%2B%2BriMcDR5GKTKidA1AHt7jgccz%2BKOusAmHIQIgaDjt9sWev25KDjuUUzr%2FHc0lNV6TnDe1PWzJATc3aPAq%2FwMIOxAAGgw2Mzc0MjMxODM4MDUiDJvsSMGPvu4GpHsFlircA2BRu8ix%2B78ggbOZhZbVnNGsIrjv2TH%2FFJ%2B7XlkpAV3fJfVNmBizhuWaLok6COPlZxk%2Bi5B0CjMqMVKFc4IK7fzqBghcOI7MKFEj%2Bf9n2HA6H3jPjgpmxa2v3uioMs9Wsfg375fIAC2EwFhabjP9BZsjsitXu%2FuR%2BuCOkQzE4YqU6UzbGi5x2dTSnoBKZImhYm%2FhhYNUn%2BysTRlpPC92o8zkqtCBuK5%2F%2BF7BONG4XFFK7GFhArY2%2Fb0lHViMKSFfeUoed4NWEBI2MiBT%2B333jHrB5F8rmY7gbMbca8AUpv7r%2FJJaJZuDDXENbi3myeA8UAB8OO3FZ5KxiakSTJOw%2BGeoRQ93pRKhKXt7h5rJr4Ob4vLHdgZ7KU15d4k7lMNgQDfSD6vGpmT7x%2FL3%2Byh3mevCVjOFjGpN2y%2F2RmYxDPTeFsj1XSjXzDPDdBbjO8xFmKxkFsJZqge6YAc%2FfPZOJC2mP8aY%2FojWO2P4id8Dob18D0p1vLus10EBF2npWbopCFI%2BD7nIxef3VXnq1iYvSnqIQp5P%2BPKxuh%2Fzn1XcOR4O6LZ%2FpSdMQ1rlGikynDEYkz3IgM2b7bHhDKLCP%2BBRYSx9Ql8Wppb39etFfCNwRFegvUI2AQ4Fe65fvnUaMKub08gGOqUBcJ5XpxDyvSGE%2BgXNNh2U4EJo42WhfEym3OJD20IdxjCLLEOUEvaZnBV2lrdBsBfIDmYyR9y9rNsqKtdA3IySH82ytOfnHWqCus%2Fy%2F708JTrtirIJuGSp1p92VHEP7p1SyrnHIudOFlzsqxo4hihtMywcFLo8%2FB0e80BroAbYAhxhQy2n0hW%2BHFJt72uytheFEInauX1Q5YpbFM%2FxGA4y4qu1bGz3&X-Amz-Signature=e94add955b37eae455f7e005ae68c56ab014d72e08b9d85aacea7c7c71a5bc76&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

