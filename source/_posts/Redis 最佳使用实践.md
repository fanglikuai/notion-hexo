---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SMRTDH6K%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T160049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCqZw6KZEmtNkx0xVFzYpm1MCyN8D0DjO%2F%2FmU7JBcUUaAIgeQfmJrC4SKR9raoFWaaQ0SuLg2ELAm7xBtpKWTNMn7kq%2FwMIYBAAGgw2Mzc0MjMxODM4MDUiDPUiASJQp1OXz53w3CrcA38DODqdcPZq1HO3sPmdfjHiUpykWTH5MgWvhvp8qRavTukc7EEcVFn7bClUS7K4Nd42uBMfn%2BqE1ABdht%2FlHnyZ0n%2FJUZggnPU8hcbgK2%2B%2Fmb%2FyPHJMmoZDvbwUF0JVXwm%2F7JrNRmHxkOEyAJ2Bt%2BE58criEbryoeeJrcitewLGjvqiII9f5K1irmX%2FzqzLQzuOHb9NznV8ygjQ2CL%2FCGhQNyqEg1M929vYN37SMkupB6keYDyBCN7DJlTMOPTqdkPRdvwovJAP%2FOYhtNwI7Ayj39UEOc6JuG6FJu%2Fvc7%2FVblcdDbAq6%2Fop3QP9azy7vTOwZAdapBOFJZxj%2FCQU%2BWLl2ah%2BTsXCVF0KfMmI1DtgnVRqSN5h5K6bz516KZhjTi78AYx991mVlSO%2FTkplHaVSksVBnf%2Bqnq%2BSHbFW4lcI4J2beGpTxTeCOdlKHH%2FokJrwBJN93%2FKemhEJuEWxmdzHLFYOjV%2FQ0H8M2sPOyI2RVq9N%2Bpg56KR7Nat4mWEpMLGxFYtCFO7kB%2FcN7PGJpCkbkOc5cQusvOHSoMel4JEkZKEWzrjCAqiTMvT3jHm6Sp6TJLWHOLxYbvbX3%2F%2FQj2AVIDd%2Fn6pKuzQoRCjDzgR%2F9xuinjTcs4R24iVHMPSX0MYGOqUBstakHI1O5htNayXl38vc%2FZwAUrsOeLjFFufwoYo%2FtfzLGa5aIti442ZmVF3gZ7kjC8l0fJyY9cv%2Fg03u9zKY6mv1TJdrcXwN7rSVBAzS6DtC2UAah5do81l7D%2FhhW4hJehTu3SJEnMJ%2FNSpJvt0xYTDukLoy4MMBU2AmAiaxQk57q%2BGWf13y4P75RHCMK%2B2%2FqnzMcKySHGtqUyBCyRb%2FguYuU9Md&X-Amz-Signature=0fe5e8ac5e64acaecd761cb6efc8d6ad600e0530110fd6b0c0c3722f86014e52&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

