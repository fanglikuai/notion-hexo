---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VVVNWANL%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T050045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDn%2BXAJqvbPwWV%2Fmk4skdY66zZAq7QnHFaU8pW4vZXVQwIgXR3fuASADqC7%2FGp4ifUus%2FIT%2FHnQUN1J6noDyuwwyxgq%2FwMIfRAAGgw2Mzc0MjMxODM4MDUiDJqOAWvKuBcgaRAqtircA9G1OwqMQnL0ERYvpmOubbPmNxfiWNBVK1eXt%2FGrBU98SR66bEjS9OmuoIsdM9QWQRy7N05tz4lieZtg7SeaAq48MjyD6Y8E9ve62o5PymQeu1CBIO5QAE6dPeWQdF0XOmSLnVC1UjvW%2BpO67V4pNk2JA%2BTNv02kICEwjp31bLqCw2vyADT1iJAfCiX2X7B%2FZ6i1Q6xmlnVC%2FUgKOczzsVsWI%2B0tz9opupslnJrrnbbHW68GlSnavG4bKICptfop%2Bn6EJDE3GpbCCKASZUdlQXLTUrs1ZuXChcwDXO194uf32WZAuJHscbBIcn1BnCCSHuPX0%2Fze5WF%2BBAJ5z6XrbWMDzZVXm1HinhVRE31YPPZ6DnEGRDKUSXbA%2BvIyMC5Om%2FA1u4TBVDfouW92GeC3XlYKhZglfGqhTGBjuW4z5LwmjO7kRQCNSvI9JYaEW2gWrEQGABYpiJMCaFKpSrXzolzAe2Ta1YBDmSwtkqj%2FwOMM4%2BQKTzOufA%2FoQQrT4WR2RJ1Sw4xbOwyh3is53BEcaxJFo8mMuuWop0%2BDbP3MIPwaVDYLM6GHREwpVu9itAnqyO80BA0Nkrb6oP6AkVeM4Q4tjW67H3Agcy%2BBCILOMzeo1pcwgqvoa%2Fx%2BzNO%2BMLTqmckGOqUBNfQpDz3D33zOdnuQ6U6UssqLrRBCZW9WI7G%2FW6A0TJogE03LW%2FORvcvzzJ4g%2FuR9XwoZ5F5KdTImzvuTWQ8VIpyVs5kwdqtse2EmO2NIL7l1drsLk7Y8Es%2F8auWrvT4FstSR8txtdq7vyuOmEZZIWtHVyscmaZlK%2F900aDB8BSUzZ6dO5ixJfn79GNa1GkLmvXVEaWC81kqd4y1VrGhWybBgWfTW&X-Amz-Signature=23eb8305041e702385719188d3e093a4c57ef782613962334110e2d5decf8c20&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

