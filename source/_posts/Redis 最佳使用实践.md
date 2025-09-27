---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663X4RO5M7%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T160051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECAaCXVzLXdlc3QtMiJIMEYCIQCWTY0spzbyCjvTmHC5eXOXrpImZ%2FlQ9Sa9kbD%2FTnNmDAIhANtaj%2Fjr1%2FQG0zWSrfhwktgwCJQakKmSW5ocrbCmFLSnKogECKj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy6qDmfLIeFvFho1Vwq3ANhTlxuorgvrwjBJ6VQXH5xUx4wZLWiA8AN6AID%2FrCDrVGD76KYZs5vXMvMzuhtxsNqpLux%2Bf8pgs9Bvnx51KxwCqyQoWo6B2DV%2F%2BVXlvPxGmIgVkf1K2QImkwBuuDlRjxgL7037GFJ87ZVyOi1bgwzH%2Fj72k4omtT3kg2CA2Dd7FL65OIWp6hzuBlyNKS%2Bzuvgv4jGKODu%2BQ8rZPMfBSEtclABvn40oGv5oNzfyoSXOQKzeTM6XxKGQbHKxYy8%2F%2BJ%2Fy%2BXTwgfnpj48vDt9k15m4jrnxegOLC6I%2FvwXg0zB%2FXKuMeVQAlAOWP%2FA4Z6zuOeUS%2FvIAGuU48PbzIZbTgV9WgSF1EqJnZtGkpbKi974TOFqrsj16IW4FEi4u5SU2k%2FQcGbiZL%2BnjM8t3BT0T5CF4Cs57AHNT30xX%2Bu%2F9iTykCmPLJRndHyZxQvD0kJ2Rk9dtzEFH6COINsB8WNNlR5HQegSueDB4HDNJ3gpItxpuh%2BgZuvf3s6Z6pWAUD6QVjCd5DreiLmwarhZEvWeN9l28d8uN20zT%2Bwjrz5samRwVGSFFYghcrJDZMVzoAPyYvYJAa02ycrgdRcOLZQWfYHlFFFFYIm1X9INA%2FIkadlDIxX%2Fdm9cCCL1sApuvjDggeDGBjqkARIxhl2tsOxiMavUFVnOBipoVRJLrxY3kEc1V4PW%2Fq5vynwhkTc2yx9SHeIqQxdXZlOwyVKEk4Ls8734C0D9ke%2FqrHGo4M9Qe9qDYouKoWk7sMH1SToMvTP%2Fapzi2j8J0yhHqWVqorQd8sXNfzaKhSRqGWqUnDXpfWvpPFHaTLGHPuMRfmXYZIWeGXaAIiEEpWsYQnznZ0i6pEwDAsDMVhJRWcB4&X-Amz-Signature=fb4019c39f308dd2c803d34b381707796fe66949217b8ae17e316cf8fb9a5b00&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

