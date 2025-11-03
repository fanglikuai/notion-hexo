---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TE4U4CMQ%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T190038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDvQ6ka6yvl105BOnldrbRX88coK0Kr6NJZznoEMrcWcAIhAIuzfPkg1H0QH17F44I4t9G5dW%2F26kyfr5mNJeI6G0xGKv8DCGQQABoMNjM3NDIzMTgzODA1IgwgF9H43GUIkBy4ba4q3AMxQaUlnE2WFM6wTs7HykZYR7W6%2B%2BC7TtiiTB70SjqmESQozqDMZ8kw1JpXAsGayVlO68mtwyVGR6muZciiVvSXOH9ArhbIWhg3dU%2FyXjZvmSZPmsk%2BAmgUHaU9oEsg%2Bs1QC4lRNMQixzISP4XHv2%2BJSRSR%2FSHoF%2BP6gtmDjnwPnkhTqKsKs%2F49c%2FrqFVDnz2WR2sTT6JGvYpDx02Ig2Q%2BHtzMY9hic14NwPYsPhY1T%2BVyeUMzng9S30bp8g%2B%2FBmFHg54uQPNilGYxi9Ti38BOfGVl3e2mhoWUchLj333vgGFzX%2FbqkVSEZM%2F3QrQuUPPSW89%2FL0TqH7jH4deZNKGFWQNmu27QF0mPN3rQnpNaLyVNmAF1sl5W%2FICR6XjwFIgFhBdDm%2BB254LLuw%2FETszXRwoEKTHe0oSYkR2Ieim8VaL%2F%2FnXcWZAQmAU5FPxD16shjG2VqqJbPJKsFicOKRv%2Bq2dJxF9cLJnLNF0en8WPGhgUhG78TyxIGQcNOIC5hf8DHF83T7csBzeOiy5HFRM8fZws1q%2FSR90N5vaOd3jJIe2grYrxGagEkC6BSu1XvWBJwB%2BmfOT7gilCjFZpCqAcecfmvHZWSt3Gepa1HPP%2BnMMANhrskcJkNJaJDLDDt86PIBjqkAXA7vm2OY0ds5OMgcwWsbNgdBIXQinx0IylCvbGc3HHv65OFT4EEXnPohT7c9ubx630zjjk89fjn2%2BLI%2BpcVq7%2FKzHGzfVi66cn3tSd66F2eGErbrsWOiufSFFchSx0x8moAxWoJJH9mONIWkZd%2Bli1NghtjhH9PbsFc1h7oZOjVeqb13s6B2Wsl0A1xToVOzr66YJgEhyG3dX5bNK0eJRbwBuN0&X-Amz-Signature=3ef3c739739ffc6e268be2cffd61da20deaac12cc1377d481f1159f91f79e405&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

