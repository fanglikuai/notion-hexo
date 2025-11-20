---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U2L2APNS%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T190039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDMaCXVzLXdlc3QtMiJGMEQCIG6AonfCpgAyXFRdbSl1Dr2z5Bf7GwQY3Hus41MVPzpUAiBTaK0sUlbEUeS7inUBZXZJ319aHZb8bvETOUaYZLsdrSqIBAj8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMwl2mxYyPxuMNERXBKtwDvsZa6sQZGlFECGeoZ0pNBgcjWTXoUzm128akmxRPKKMGPbQOfN%2BJ5%2FkgUSUmzpBGgnzF%2FzLE%2Fw5RhnfEs5cbkC3XQhH%2BzUuOE9UohXZnHXoeA%2FF5U9LZe8n4gOuWa78zC6EYYknbR7qz2lHqkNjGhir0x8S5j3QjJunqRzJ%2BTrexVe6E6ecmeaVEHiBCG7tJ6Jbp8QI8B5dD6vHswbVcSgGBCvHO9CEK8wLEZBIMm76gU6CZrNOm2QhLQIdOMbXgTT5yZSqNn9iM6oAruD5xj2rqckKMZ3aoR3531HFYLFB2yvyaUXaixt687%2F3H1pb%2B7HM21yvPvMx7ClRPm6M4p%2BbRVTuupf8mDIhlVv1bN2XIoLOa%2BoHznTqOf82aew8XhMTVSzU4ZwP17pNkloWx2BKJrYAomxHt4khHbq%2FG8%2BXwpAbRIx2QmN7xr0Z4wxY%2FQzJpwOALbYVF6JHRt%2Bsk2ERgo6btKMs0JKVkmAqZbpHpwrXou2ckZvhH64LJZDEWHa8%2F1ZSMhO7%2F%2BfAB65Q5PCJwYVgiGbBkHj1%2BNBOSpAoEPBqvAz%2F7y6VUg7sfjmzJMqC0k4lm%2BkD3R%2Bh%2BwnoZ50NUehoE4n8UF%2B0O0TYgaawQdTteFx2Bn%2FasjLMw8MT9yAY6pgHKy7FgOUVb7u9lZmENKWSVS6DUghfpoSE5LtXd%2BUdqlpD93FFaTSaWw0iIropWrT0wZIvpruUoOsysQHfcU3WUqa2MzvCSGYYukxR9R5a3s10%2BCyQr21jGByaUYp9OntnKNKmbsAPNm3Zw02HBp17W37O1mfUy4m89I9cVSKJ0KbM8piZcUc5UYwmFk2uQV3CPf%2BUcd8PnS96l9G5jQyvMtb5MFYST&X-Amz-Signature=6e493fa5305ca76127d84f42d73bfa8592a6d748ecf3ae2705d75d40bd0ee1e2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

