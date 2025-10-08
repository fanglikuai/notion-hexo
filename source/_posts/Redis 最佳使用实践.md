---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VAQYVB4S%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T140050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECYaCXVzLXdlc3QtMiJGMEQCICMZTYXBiBMZG10WhVRz7ouDZuxjOIEMxAcdHCVXUApNAiA1RSCZ6vDOzr%2BuOLb6eZHWHpc6oQS6R8I1TpnBYtUEZiqIBAi%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMLZNCTttdD31pJoo%2FKtwDxbioRNMEoAcqin8sZ72W%2BoPI%2B7ZgQZeNOVifNnEEpWtl%2FDi%2B7%2FIGvxL0oQpMwt4bfPp8Xjex%2BfLO%2BCvAHJG4Anu14GYqEzMKwi3DhxEcC8BrV2iivr0P%2BTpYr3JqZkjHccCLDPHpHPEj%2Bf38tM7ldCbENTjQww85ksL%2Bqi1h5N5zpspJ0yqFfEvhOrs1dfG9M%2BMnyEQomgRqFoEavMKpGXOk6uVQPiI5vzxehOGar9WZhzSoEIBxhU9xMMlYCBRV%2BnTkIbAt6r3JkaGoAba%2FO92PZzxSHI%2FXceXMaLM5bJfADiWNFt5XJfQgDDMhbgVyCEtqJaD%2BDCDRzqN7%2B6ZkGeG34%2BLQ%2BghlJYr6mUgvkjtxtVHhLveB4rGeHQavsnOy555OkuzGmrTPLg7H5HXsSQJ9x1WAR0oFMttlJ3g24ozFV9k1sCZ%2BjBBCxdnbVr4jeCTc51QDoDwB9BtNLY%2FBcPSQ2swYD%2FuGMozcr7h5DowGUt2kPgrYfBXCTq%2FE68hYujM3bsKllO3L6hFxKfNjBrBLyIvMoK0AuDdjiDgSFMMF8UlHt2BxIWxQ%2FKCb9%2F34mtML38hGr6rYljvuufoAjM0SVRSeIZn%2B0bBmNQrxbu%2BHd9gKTPK3Nox70hYwj9qZxwY6pgHCUzURt4tiWHwcHzn23zQvgQRTkXYbsug7YxgMRxzHqsmIFW5y1Fk6C2n6EINkT%2BfebPI7phsQqsnjknC9IOKfZH3RowYJzfJ8nm4nI2fV5Av7nd0WMRIlrm6qDRqaMEoQuL04A1c1fAPmPvP979M8ZA947s5pXicNnDqsDTrtJ2Ic%2B6crWuwySyl%2BUWmY25FXxV3BAByklQRP%2BFjC3sa78iB8ddKK&X-Amz-Signature=078857af1e00fc5129c8013516f50e59afe53eb5708c13484eb51145f76682b2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

