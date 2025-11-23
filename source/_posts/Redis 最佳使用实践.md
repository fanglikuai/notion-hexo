---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TN4AN4IC%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T070058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJHMEUCIQD7c3IWjRG0LjPmea7RZ7UsSTIGuiwPy%2FvESwSSQIL8vgIgM46sEsiDd%2FbSzWk7s1j8LIZ3e4LDjBbCPpaPXTXNfhsq%2FwMINRAAGgw2Mzc0MjMxODM4MDUiDDN09j%2FbjXod7SLELircAxSlLR%2FO2717GMYRWnc53UP7DmF0ItL88W%2FJG6dfR5xfUe6%2FMOPNiBN6gkE7e7hQfmCnRsGILH6%2FsAE1QDbwu0Fiit2bNumEULgVZWpndmkDWR4ElTb76WS%2FOKla9EFkk3XXGqeNQrQqQk3NK4z10tWMM9cmI634UISMHaFrKSUO%2BFW%2FDNVM1yFbup1uTS3Kre1qWR%2FLtatB5srh5NoozEgLlVRtEBkJzIDXSjOU5mU12S8GZUz9xzGojeRnPC%2BRy8CuMXwYsa%2BfNvD5xZ1ptUlf%2Bm4DmlP4ZJ5MmgqIi8qB4dV%2BnMAGPL%2FmJLPn%2FP9hwIxIQKx0WiWD9T4HiyZ%2FFbHvqP9mxsrvBmA68OUvbqCO9sMNB3SJ9vlcIoJ57GWcKGeIlf56VpeWdqFIQruqhtaTcjWM%2FFLBKYq4FzYP7J9h8hToiojft6KNoZa73KKrsgSuu12OeJC8gP%2BYAcnZq%2BbaAXFPpodTpzWCyYG75WzsM%2BbWWHjLGFyjeV7QAE4UExWWgpgheqrKw%2F%2BSMhSzhNRwqP77tLcq9G0f%2BbTVz9IdICbgdH%2BhKCL2aNsmGygB5T7n5ibmJFx7blxvYnIW2onngas6dQ3gOF%2B7sQ9UkaXp5B7pRIVvOd7QKNLOMMmPiskGOqUBRDHgvkfEaMXhdafXjs%2BYgF238y4OK5yZzsea%2FhTfqOoW63dyBT0ALWVpJy421ReupxVy4Nf9IO%2Bk%2FRw0X%2FSq6V9ISczTIvNEcQZv7kNCOjUJ%2BBqoS6Ex1oLWJwxJ5bPXtH8WlIq3Nek3R0mP7KjWhpbPV0w7vRmy4XRSb5tlqDYKpbHPWcaJOQyz1IvVezdxOtOLyub5xcu4CsnOVpi5pEQ5Sjf9&X-Amz-Signature=68b919265b188191c3be63f03af9f00687e10c42a770397e6f42713eea0d7f67&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

