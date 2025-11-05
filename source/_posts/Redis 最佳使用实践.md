---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663TZXVFUW%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T190037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDSNS9w99tJV6pVZh%2FDEEtSAq30bfPFNqHeN15jG1oS8gIhALAZJ3d8PyP8H%2B1H%2BkIqt%2BNhjk5CFbZHHAdQTCqV%2FWs0KogECJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzLbYBIjPDJuqhWJmkq3AOBYewajdE0yGhChKieUw2oW9biSkkEVrCnj1Tn61AH90uafBTHdGxnY1hSQHmCHxwrmfWnzYUlyJ1oda8bMm7DpeWqidTnLH26%2B3%2BVVyPeApWZ0HDgLzaxRyHdniQ9y2jz8zp%2B5bsAkHcUJHvXF8c%2FwAV09exqBhE2AUJ7K7pzxx9f%2B9nEbcI6bqdK%2BD4RaCAtt6MzWxDOjTkPQFQ5TIgoD11JvakeTwRiaOFKnWW1%2FSyn2otjLfJIGbZXz6%2FHIOXMvFyFto%2BnwwO653BqLQZGs4bWMS2xLwth6tQJ4a4jQRqT4Y9QI%2BR8nljkxei49Gad6%2Bu6YmzUr1Swsa2yqIv3pmDf8yaBpowTfFaPZNtHZeMiRpzM%2BU7k0cwpifuP5LyeTf72cpQPVPm02E%2BABBfB8qMVs8ugQhbE%2BbH2biS9V4dPj2bjp7KqmVXSnSw%2Bj3pUDVu5IV8sz6iIdx7ecZEqXatM2dmdDdu8ncugqtPbEDOzkh9k%2FECo3C%2B3dPoZ39xWxd9AWG7WUyk4TfmS5yy9%2FJzbv07Kwjt1j9SfcLFOIh9cPPdD4Ek3Z58XMezOPhlE97MxIsNUpVEXB1l7E%2FlXJDPefDqaX86rVSACdCRae4I2M%2F%2B22eAs%2FNjmhjCLuK7IBjqkAfWqCZqqDSLYpVCdDkAuvNLQ%2FYjwifrZ4Ojm5kNjYx6LmEbD5pGs%2B4YZnhWaUgAmR99aahZvuhYt6ul5EH4A2vSvkEoVm0Adwa5eZPa4CggR8%2B%2B5GlPpp4kPPLIGpEKkKj27NzQUyayG5V2ey1diIn9sRSOWL0b4IbbVXqBuKuGpjbchoG5uVXANiU1gMDdRd7h47CSZFGJuD0qc7UYQSrS7fXWO&X-Amz-Signature=ee8f4b7be3054cd2641b69bd117488fbd0f3161ac4ce3a9607beccf696ab5308&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

