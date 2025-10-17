---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TE6VJYQH%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T140305Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIE0T1I0FQJy6dnjZGKibgEfu1wKWDFy9FpiEdfRkmPiCAiEA%2B4SBXM5n%2F1JiPpsOfLVhJcCI%2FLp%2FgX4oexzGMRqdTo8qiAQIpv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMvJUY64NzXJI4HM1SrcA6D9LgmWmOBD4pwlWNYETjcWqXas5VEl6cr5jQ2jlQhwCvablUuPfpq8j%2Fjm225bi2YLwFjK7EiuAVFhvUPZDg43%2FAFA1z9zFPDdAtrvM5C5xxwuxkD1e9b9u6XbaDh%2B1nF44brIhnLmFEsEYp5kZys2KgLX4%2BN9RyUsJNsJf5A3iIuix3aDytf4rMzB7BJiKSSojKxYvGCrrujrFow14hmM4ASc5w8jQz%2F4hwtisp%2B3uzGyuhDM2%2BUWZX9dp0TjXHCBbU%2BYbX0asrDv7ioxZqRYQZdcvCRLLHYrtWyRudbWMDp0429T8cMxdtAasfexJG9q56xX4CXOgC6xWK6gVGwu%2FnDzJIiHsji4pDwBH6fPtHSeCw7TyEynr5Z4ypM59x2YNsKPZt2%2B8uD99mknUf7mxTNCMFdnqRWFdass%2BNgx8wegcbtpgpq8V3EHvRDS17a0Q0LvaWuwoSNujO%2BRDGqGPdECshr3hxU3rNyhUDtaE6ehmXqaB8YM43G4YaeMlcZivKzIMhgQh6s2HjrRs90oeofwxqnkw%2FpHbbCnlnCJ0QPUNt7f4y2kLMS5Yz%2BncYAQ7j%2Bodqvc3hovIa8bdvYD4me8hfP6DUDtM%2Fv24MutR1o0yh%2Fp8uegMEYwMNSByccGOqUBQiYzV%2BKeROGnRgryHIoOHlZ4S%2Fj25J5UJVtORX1sAuW1UYp4q%2F8bKY8nnpqF66OvNgRXvXJdyczo9GZ8VmS1MugVkK3NhHLau9Wa9X048koTfWt%2BNftkKQNcVEWq9wOB22xFElWlvAo3AkGla2Bq4sHPnrz9Sh%2BvwfiXGcxC8H1kpzG9HcC4Q1CVAgARmGwEiUqtZWAieV6XVGwdmoF0ywf4HnC1&X-Amz-Signature=314ce2a94ce37a5a0188fe8f6e27f2635e21537f7dfb5a08070848f5a25a8be2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

