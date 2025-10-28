---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TG52J7ZL%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T000054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBNeaaatuQkQeHVCj7FGwMHxh808kJAm5okXwvs9Dwh3AiEA3MIPiyGik8PhxdEfYFrTvj%2Bm1SfkNetXGC6Nuat1QUMqiAQIsP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDLg9YWpg3uoVp1cYCrcAxKAE1SZWLIxmU4SdBZIZAFXu%2BnGMKqQDrRBQcLqcGs5Rr0UL0ncSYC2IjWvTjPjJvPgM4dA0lF2lA%2Bk7z8r41TtkSrPe6d4QWjTsp80ShQG6JIiiTWpKOxG5KoRziZA7PWxteXg12otwOgEzNNVe5ndH0phLtvRdLGqmhXnEsCXFlrMfny3kIQcreg9RGL%2Bvu6eAXiJVMRtPObPjEtH2vkrm%2FZW3ZliMK7XmosDNKCyRTk1%2F6pFNijaIKFBlV8ZHwEHjl4Z2EbDTwMEjgoUEpRz6%2FV98IeYHefrmRoZd4eeHVGHrFp%2F4wLu1SEe28JxVftbOBoYfcdzU%2BLvoj8264nllWdMIPMJHfwUwZHECk7Kg6VuXidBFUWSD9nPMGDKpb4HWjUYN9mCaxvT0w2p0b%2BKCidX6ywKU2amoKzmBXBZYmWYcOWutd1sCmFkH%2FdUM03VAkYq6U4ljcEMJLI%2BaeO8y6uoZLam6BRGvlbLtjud4FxrgA30nzsFpmq6I8umzlO3hAZKsh0hGpqB5pHGcC8WAIQhU%2F7o6ZPbHMUPtesmt6o%2FOvwhM9lZHwvY0gDxwH4bX48BIbnQbYcBjbobhmlHdWHiRrck6BMnnayoe%2BMhE8A1i3pkrRdCrsXQMPD7%2F8cGOqUB9bKF937J9f12RKo2Ak08Rs63e0rrkF7VR0FlF45fr9Hl%2BiecIYNpRT45Qhz0ZSQnvt9nBSxzaU92k5kPtLPxzLASEy%2FkNq2eNimqXl2aDcqwdjpEnacLN9EyIgum%2FjiZMqrpVJXQyCyInfjozfZEu76AtY1Pa4Z81whnRqmZlUYPFxxzDI2rblzIO72%2B%2BfGUqkDklFd9FRzFMTPFPjNo3JIPn0zU&X-Amz-Signature=434d1d905e8350d04ea0e2e4d6486d2e7e707718a03fd533483bcadef6d51e1c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

