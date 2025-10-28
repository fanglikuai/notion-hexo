---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663BGC6BVM%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T170103Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAgaCXVzLXdlc3QtMiJGMEQCIE3FYaBGpQlNuOBptzO7YhelyICgOl%2BGmvLO27Rdd92AAiBM6aY8iuEW%2BEn9jYMrzzScLiGaqHtr6iIPDEs9xlAGMiqIBAjB%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMsev2Mt3uTHUXxoiTKtwDoA3Jm5LujxchwOjVR4WZbjn97jWPdr%2FxgLIfH6iV%2FWK%2FF5hYQW2bTpIA5J0TsXOzHBobyFz4VnHpGCIYMZ10nHQpKyJ6lGY1oqMi411VRwsThKkHfimLb8M6WNYyZGTb68BYHDOVT7iA9g0LBvbaZkziWuy4srhJ38VsV40pUcH2W4sPuulsAm5k1jgfOcXkFFU3pLc0fjdUWO1Ga9yRHWcCw1RCwa%2Fsrty%2Fh2Mmkfs8n%2BNbBnV17TdoUmznc8bBFJ82mOYqyDPiQCF8MNs2OvzIw%2BVYqS%2BAegjbQEVZ8JCRGlH8xRysRsijdfP6xMf9%2BPsSPfTPwnBEVyPSq1eyH6BIUil6odRR4DrPWWkz9sdMDr3Oq3QL0%2BL2B%2BxOtWcyZWueYA1J5xAU0nIA7V7%2FMgLbA5MvDOwbJCNJ4%2BQq%2FPHARXtLJ6iLOW2p9UMVhuD7g3CNFHxyFq3gVIoT%2BSke1g9lf2LtbC%2Fu4kfcKlDyNoboo%2B1pNZMJHzsT0o7lQt%2BHkP%2B%2Fg7pNN4fjiNNLewU0elfeYofg0%2BKPFiXjDZfHQ7MtagjUTDX0DFNmXBd7O9g7ZsVeDnskGWNidptl3y8dQ%2BlwoOmfYGv%2FE2WaocS2eO6e9aOo2fkgxOB%2BnY8wgNCDyAY6pgHxEem%2Fo0okU2bbzf86bfuclfkvzbcE3qxAIzwM9do%2FkJxerYTjFysE4gbh8m79nW4zWNDj8ZCAje5dP0TVaahthfMzTTQw6v9gqTfkWegS9hFUEg5aQpmTSDQO0kGbk16Up6n4Z%2BLAfB7%2B%2Bsk3nWY8ln9Aep%2FK2T9gA3dC2lCN35316ffVsI%2B4URtO8GJFz6vE%2B5TgHfSYnFScIGf2d%2BbugD2Vge7n&X-Amz-Signature=aaf9a36a23d0641b3cddc4ef8df3d5d05f05672180ba7a7ccd875f71df1fc4fe&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

