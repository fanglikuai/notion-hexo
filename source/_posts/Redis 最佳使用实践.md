---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y3OS44A3%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T190043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBCerCjW3602lzhdyoVfxey6NPMlZW7Bbj04Af1csxqMAiAgWCq9UMAurNJ4KzfipAs%2BhF1A3jBhyihyTpDgxoMNliqIBAiT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMuywDyOOyPC0B2xChKtwDTbunwH1yQUkRWaJuPNUU7tKESqsk%2BzL3VkcHqZYz0XXES5yJFjzbVt%2FQqL5UomZizA5nJ6R0WS%2FYqMb2hAlpBkMVIOSvOAuxP1t4oROrk1f%2FyVvI%2B6EKTaoeH5G4SZE6%2BiGqDwchTlRlz0ZewaIETX6ldESr7iFig84O0o0Vw3%2Bh1YN5gcjHVhSkoc%2Faj5X3Dc63Vv7MlWmPuNrS6cR0zBECbNTr4IpOd0WnHnTq%2FfLEM2XKOOIPNAoFFzacKP1nli5JWCLonSsT43HohCF9AXqCntTIj1C4k6EO9ON2hfojJTbDWLA3XOi6ml8IRCeUAEs7qJIbv6%2BZufKCIpBQqqhFqEFwM4HivrCbZdARFS1CpTEB5UZnhIk%2BVX%2BE7zU4%2FeFXwtWct7HR6Sb52Q3BAYdWbOSQTsSd0vssz4cEJ7GVePYaq3Mbai%2F2N39JrjI8KMGO7CASpEehLZ5pR9fKhatSm%2BF7PO6aKSvuWvhIfY9IYyblYIxRDxQcs6kRM6fNIivRz6XpYpORlN%2Br0cLmR7qggSqdURsREbZZNd7D4C%2FWS6YwxFgz%2F21bYw6rY4IHbzS1zLPR1n%2FFcz2Zf4tV90Vs0CP0cg0s4mO3ZBos5j12Qoh%2BLSGDI5MHHM4w%2Fs75xwY6pgHPb%2Fk0X4kW8jwVsztVvSAaGZW6NmZVKbdvInMrJxioI%2BFYy%2Bx%2Fx3HIiiyE5BO3yQ1wd9b5tbJ3QpJOQva06Ir2z5wEKQaiDnGroYPyQTKPSDxr2a7HBbCoYtT7nNuBOpsr4yZ2pNEBFHxL%2F9cWwoZEtZYTLoW30JZP9fxCArJNxxffT%2BB%2BVmV%2BxF4odPIIbTHm%2Ba2UtO1%2F8nXPiDcx7xBlFQd%2FTGKQ&X-Amz-Signature=3c6d16a42cfe548a58e7205bc6df1850a5cf42017cc8cff5c087a984a1ec88cd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

