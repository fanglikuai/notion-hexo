---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UDB6DANY%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T140040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCIQCF9rY8SENEtlX0NRGrKS4lJm9kNhOBl7vODMXcBDUcBwIgWViHW7nLk4SL%2FE9lZjUXCBHcWHH2xxyyL5eEtHrL9hsq%2FwMIOhAAGgw2Mzc0MjMxODM4MDUiDJy1ZmuZVj3Weg9dQircA8ajgoeNjO3cLTww2b%2FBKtgcgmK2z8xLrl6J73chyXhlcRihhB3BJvXmR%2F%2FGCTuRUoo8JOkYAMUzfq%2F2mMW501hJkyEAQtwSuCPi3OBE%2BIIYNP2sFaNfQ%2BdesLjvGGGMIH1PVuCz1LD63qG7hJzEwpEETHq2C%2BefcjRQUqOHkJxkG%2FA6mkmk0lRUZHflt0AaMMma%2BenffoLYXt0Wd%2FoTJy%2FB6xM%2FkNKur0vCjE40I5Hsu9UWsHmbLlYUtuuw0G1MY6fhrWqjI5JtuuOPf41VUERGHNSLrfYT9yQsJly7vSsASNC68HVWpTumAf3Ns%2BCxTc3jUXu8pYZMMehfNsf5anyoToV%2BS8GR%2FLLBW0QRQiURDAWqgPYe4CmwZdd%2FsvzYLLbR6bu%2BAxXbhN%2B2TOWDABqNdY0QRbCe%2FcjMvEPAI9Z1P2kUwpTMX1ysslruiPwYNYbOJHM4vHYFa%2BAZ3ruLoxJhTj1L34t%2BW2f5KLadi2jKC7cWT35MkaxNocGGqFOHNRLaF6z%2BK%2Fi0yYtqoHrK10L%2Fm4TR5yx1jozK1CI9NmCWgj%2FSI30peW0vjU%2FxgNuWOYRmOy2vHclx4QgZPLwqbmmcb%2BBdXw%2BYlfmlD8ZP%2FrcgQiYedgZdKK%2BqlQAxMN%2BWi8kGOqUBhe%2BcvfTzReIxPdNxXTGM%2FikHiPNlPHYpWnn5EVUcVm2KNHTYniHzUmolrYPR%2BmhsJEA0dHLab544Nxw1KeE4LNFTNUTNm1JX%2Bag1J5AbCnN5rBGLWwbcCqOG5RQ4XI3rcpt%2BESLbweAj9S8OcS0LZG%2BsZ3lTIVIrK%2FcSZA6YfYxSn956YdBYNx1C3rU%2BlP9WbGiqwKPkgEk2pN7ye07KJCE2UDAZ&X-Amz-Signature=46f9389f8112a4ce9864bf44606b7df7918fb1dbbbad7c5063514def8a28bb6d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

