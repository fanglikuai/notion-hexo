---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U5EHGZP7%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T040040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEhnuyX5I3VvXEPj6MMs2JMrsmeBEvI5H74hGgLdZmSKAiEA5BXoS3rahoGckWaEbFiSDWqtgAcHJKFuzdtJ5KwxyrcqiAQIhf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDffmjDKqO6PnPuBYSrcAxnlpAwPevDsHEmuVjpXmcJhqWiqB6zLDmNQ%2BOsPhndDf5v4x1ucVeSzAcYLXxMxvAXyMB8t0Z2tED8ahFzQYkRjx4T%2BTWRovhOCy0jS%2BMKlrUKKcI640LtJEhsAEILa%2F6IJbCN1LkCo8CEaKWQVu6eD0B8fbkEuRDs8vxWBGUxPXmfN97gTApQ%2FrLyd8KeKScpOE9XnjJysG8PoOHQt0SwpWMqlXeW0wZzyTdnKwcZ%2B2u2cILedQ1ecGpdVgbxx4JmvI8MS4LFluRzh%2FJXedC37KGCYXRYqFLiXzmXizOqxUklN5pu6bmFyYTIvqMkWtjsnif4n8OtPb5ECzkeydg6c6i5zbMzktTvRL6%2FqyrASpbYuqGfBnS1JEMHN7q%2Flb58wDgoirzO%2BGAN68uqmWFcqOIygABBFiihLEGWzbYNWiNI1disLCLLWsjE2WXWtLjGtVJ%2F5ieP6SsQlrbp5C%2BhtoRtsvhZQwtTXnoiyqzxCjrXnm%2FCkviKtWHA0F4mjDjteJKXsFvOYH6Vz5E14Md2rSQCm%2F3XnpvScE6ZnOE3tZ7XT2Dwv0XDeY9oMz2vx08RPIo4KVlN9IGZVw5EgBUgjWSbcFjYmhXQsdawX3I0lLV0RoZ%2BIDE0aK6kdMP%2Bd2MYGOqUBg%2BO3E5P9csDiLLjP3O33QHycWkPLaysVMjRj%2BSJjFME3o4V%2BRt5uEjE5YLCroD9%2BNkqgnrWBW0kuzSqqHJBRZe22hzu%2Fv8Q0MXxTQyu%2F%2Fth7G20vIVb7TkYGoSjI15D%2FUL7X1WYYZualAmca6xkNAJJxIRB3pn64rABVi9i8D6Ug4IFhl7BGmeNpg2YAyDFdrqDw4gGaBV5PwqOakJtzbAJb6NXv&X-Amz-Signature=44933f09211daa47d25c7059a3a97671358cca37b7e81555b6b4d5030c77ba31&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

