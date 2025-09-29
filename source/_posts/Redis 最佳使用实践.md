---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UDOPS4TS%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T050041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEEaCXVzLXdlc3QtMiJHMEUCIDxIa%2F%2FIrZp2xRHTZK3F8gEjQzGhhornFLvQJRQPkj0bAiEAljx29AEEjCcUmw5L3MxiofZKUUx2VfWapPi2GE03eiQqiAQIyv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIilQjwNsMfdNcx1NircA2YI0tIOhPrx7CDRGDKuzquzhnW0Ahvcxbnh3CU9aVLAr%2Btq5qTliLGe9gJpYfH67ai%2F27YC6V2IiW3UmvYn2pCvexNQYHrdPy2bApO2qZvabHQq6sdWvZsn1dKX7Pb4LLcK9tFZcIv2w42SHucLfGOPRlTyujtC0FB4XrmVyTwV5ImXHUf5KE068I5k%2BsbZGNUBgO0vJi2HVirs4Q8%2BMsThGEHMKxCclb2IRmZqlAl6Z3O5GbLXodmaTL%2BdMHKhhV6b%2FhqaR2alTHE%2BDgp6yx6zD3J%2FzVxRVx2rdmftLDsNG5lc5DKuN8PWeCsid0iFfaBwh6ihAjtN3h5W0BxDaVzYbeaWkLjhOJEavS9d%2FuiBKt9F1EY9FMBPEjJtXofIDUk%2BYtKBM%2Ft95byKs273u76GDsisZayZM5sJnse8bnvXboKqSY2Ov8KS0gKuhyAOOJZXkL2AFInpObDhTlYHXalwD5IJNWr1DADVtLXA9hkyvEh4mIhdS5dSYy8w4FyRu2%2BCGKaBtej086z%2BHwEjPPwL5ljAHW3qsfHE2JV6Q67d%2FxaQrx7KDbJquiQi%2F69DNp4c2LU4Ddg6rcYpGjFngOy%2B9hf3KrCho%2BrtXfmFjbUWmfeMK2fZdr%2FKRZI7MNOq58YGOqUBJV%2FsbFP42cJf9sSd6ZgwZD4xO3Z6bavg%2BccrzGU6qdtKrCZDzqLmurpDbRvbGP33pOkfGOCs9hVbDSxXBGdiBcDkU1rGcwoBvREuA1zNYPKaXKBWN95IOZ2qQzv4fsbVxvfaBp0XGbT64Pk%2BbjsqI%2B%2Fe0o%2FVgdOveR3YrrdZYy4dRNZa3vkAaPaBgfUWaicoyV4SExGDWXK8PJfv3NjCAEozrz1r&X-Amz-Signature=64ed96ff837e3e1402b69e745de1ae088df6d803a18fc70498754e5360e89403&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

