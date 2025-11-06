---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XT6G3Q7D%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEG%2B7y%2FoP22FMK76TCfb7obGSEjDw6cfJcB7qb%2FvU7kRAiEA4Tldhswlz7olPCQPN%2FiVPz3yMWVcC87%2FHkHXkYUV208qiAQIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDH1xiNqHn82DjathsircA1JzVnph9ZJdR0CgPnV%2FEXQLb7GSMFFzLpAMLLkb54OnN7AfMxawPzSJaKTtFVjPY3c%2Fep%2BG7UQy5%2BDT4%2Fs%2FW7puCJCxF1z36aF3InaHgTqEJNRz3rZH326RQQvMjQ8p4gUeF3R07VjsK0WJ%2BQ%2FURwKa6%2Ba%2FxltGfpKsmRJsA5gSXGjHjzrhmwBV7gYRe9vMxhtiT1%2FYuSUj%2F4YGk9MaKQpbAINox8eXkRwtLsxztCqiyPxFcUbzUdRXtW0MqqwM%2FlF1dwUyteHhjBOPpWDrYYARB5TX%2BVORjceFNprmLerhZg9Uq9x%2FmqUc74pGih%2F7PIunAZdaGIuirycz0GZB%2BwRkU4iZ7QRmlYu%2BWiktH7jfSr5fX3Pbu%2FS6Udg892lro9IlCWFsUzTSk8aqy59WE17pvwz2CWhHIOEHre%2BQdKPeQDYSOXG9BWrHVNXOobhrnCc46fH9Z6hgx7HAm6NqkunWECHo1ywYxEtSyPQn5QMfdxfUs8EjucuLcXG6zHCeZr9yIlEcWd4Fi9tQtZqQWkoApwmiVqYeWctuWtUvgACP4GygO3dX40NoLLHJ4u0jETRiKZwB5Iq7yrrOoAWptfmZqLL3PJ6w4ClfmJfAkEkurhV0kmtOcuBgnd4aMMCktMgGOqUBGe1yMWdulZFe7%2FiVWnpYLMTtyeguQ%2BroKRS1E075dcOwDRKJhJ79qKmuuonk3dQjEnTN6QpvwaiLKiuv9iHP0%2Bo3e3hnUQJ0sAidRiagocXbxXsgbq9TKEwyAxQ%2BgJ1jU8h17tYYdzhx26NennFcLkLy0eSMyAhiGGbkEQo5Bs5HaGGcux5qmERbHHmC8xVz%2BtmiEcTfh64XnVozqLbgTowmLPCo&X-Amz-Signature=701b0d2216fa599ae01c119c0dbf50cc75287af5d9363f9ef70cd7b6ae4d97d6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

