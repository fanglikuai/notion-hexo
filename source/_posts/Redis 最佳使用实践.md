---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V6YKDKKB%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T100040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAIaCXVzLXdlc3QtMiJHMEUCIDBKOYPQ2gdPRurI7jBARQ5oR%2F2yFc7b7mezmMFNQ5W3AiEAwtUb0n5AWhoQlhL74Dobee6hplaOhyXXNn0ucbenpvgqiAQIi%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBThj0sZzanCid2FRSrcA58gcu8%2BIg7hcPEQf8o4Dc5kDqumhx01jC2Fg5DGznZLieZOPgclcecqOiJjjhl8H05Y2dwUBlvMzIbPmOj%2Fx12Ns950i%2Fz4zqLW5jYTK90qZSq0hzEBTQ3EDJbUEwBbFRZ2VK9GzPN4MVEXsYtECQqjYgWuBrlDT7gMrg9l96c%2FZT8GDtiZjKinYJxeWJOgXuW7vu4HdH5WrDaX3dFrN9Lb%2BMtTWv%2BMlvY35qePkM0rAmPQv9wiyvZSUTIeZqD%2Bz2TL7Y7ZMZ7zByPpcpMQBxFi75vYKcbMPOA00bmDHMYS0EKzJb1B1DNg%2FQWTJXn9qQlHMnZV1mof9i182qz%2FdbWBMNJSAIhwav2wDkxDsXWcbCCoASqQIAYDRmDKr6sPQJo6wQlIrckGa93FyZYt9jVfHiiK8elxnh5DeKj9zC44oSr6vFy9bWUPl5r0yi2AK8f6ofvJKtheE5ltgEYMIXALjflCOZO49IZCFm2zOQsXHrSBHmWle1OIU15yWwhW46kIKUg8SUJAW28I1%2F2n4%2BilBJXAdOo4nmuLonfyGD7BRwnw8L4ZY%2F5kdbjN1Qd3VFVExHWqJxCQPNl6ROT2rLSd1W%2BZ2pNCJhXCc2lL7YRVbJBkup%2Br9e9kq9jgMPi%2F2cYGOqUB0Bz4CEXqHJN%2BiReh%2FmEAi%2BMno8juvRW0UK49sMRpXFK7d4cO3XF2oIg10YO8ctlHw3i3HA%2BC4wcYX3IvEqyzRSHSC4NBdK6%2BqLFZejLowOzsW5YPNB4NywfZ5npS0YvrXId9wwaqBYBchfkKUHUINsOsEufg6ruTgIMFPwaWBKXsQZT%2FSeYfbRISWHi4vNcyK3o9oERqsnlKPaL%2FIr1l3hFt%2F%2FAm&X-Amz-Signature=4d72ac6393e84bbe630963389b61b7bf22a649583c3e2b4c4e8bc59c7bd22485&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

