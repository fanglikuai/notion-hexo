---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662QYYDIWM%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T110054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDHNbAVATw7siWZll%2FuqpKHqHcKy4c%2BvCbE7ay%2BjG%2FsiQIhAIBk9HXtc6wfmNUKenHVDSh6XejY7BfeHViUdjqa3yGeKogECLP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxzcZJXOtSK4qmeTcgq3AOtYmnKWIvAQ4xufGhIFu6LlOD46fCVcumLNFtcjBLEOWDEtx4Z44UJxpsu3HNYLil%2BfNK5FUSGrohAyfcyU2VD4Bnb2he0IYlmV7Jt1reoySZ7e0k%2F7K3zO5458yA%2BoEQxgQZ68FxV34l420ff9EkIwhpIFz13YZv6jzC0LKEUnZVdbFE%2FxmUAnvnxCK2xvRhnJ5GjqzRqK%2FatCERDersxKxI7nCtYykOU74I56ElALU7Q2vpWmkQx%2FmFd%2F9559UValqIl92HFQYPmiL5VYv%2BsEKqBlZK24Rh%2BUyKe54ryOegwHNSQICW31Uz19x33cSlQr0fx%2B5c0ohXxk0sKsRO83pLM9rppBcmjezJTWUJ7CmNkPPO09lcOPSgqMEZ2vOTnpdtX8e67aFA4XVYAqfTGH9b6R29V%2BeFkzujsMnVnXKvpVuWF8Ic1oti0ZQxAeAWM3J%2BkIBrmSMepDNgzFdV9mgkIyEe7dWPVKwflvIiHM55pa62SGtFsJrdmnh3otk33gnNPzzO%2FQl%2B%2FERCt%2FD2J7MD36Nq8P3Dn9geR4Tsre%2FbtIbSW3JL9jH3utEUnfjpVNJoY6vGyYgc%2Bzpcfc%2FcbkanAFIPlyrdcmUT%2FK377HAhNvbLFxqAU4GfeWzCe4KXJBjqkAamjKqwnUHa5i3%2B2G2DVmtDU%2F67sWbm%2B6yKlMMCloDQ9UVh5a9xYUm8HnvLl5UoqPNPmnf%2FS5wOGBfRgaSNTw%2BN%2FzB6eueK%2BSb%2B0ueZ8LLveChGHxoork%2Fg274Vv3vKDgp4xfRFI1gLtnqbmi1wsqKSJ6duiSYRH7vCI5Mlq5DxmT6bcFZshSQApY3YNCC7c73d9lbg2ZYLFYr1tYM6WfQ9BFYrj&X-Amz-Signature=3221a06b0e83013af24968d2646c3381cc4fac97c4340239539446b69ebc112c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

