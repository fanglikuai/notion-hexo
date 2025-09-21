---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666ANXRON4%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T060046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCuXfc3%2F3Aar%2FtoT9nIhpJElIF8JpKOcDhHVEyB%2FHwrUQIgZFtWA9Ix06ajo9Jdz5AHLDJbliwdbRMa%2B%2FKjknr4G6UqiAQI%2Fv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDC9lFG9waWlwksVxUSrcAwhQJr%2BV%2BzY7b73vfO82FZ4RgvcW7fZJIZ%2B%2BjpaaluxNTbgqvXHdEH4waNK0baB4vJuSGR3oOji3w13%2BM26mOkJ9GoZkUtNDCcjKujDRFBAQalYLhnw7ESgmTZGGoAioiAeiIT%2FJdPKCUfqgoxTdSV9Buh6qqqYAQZ5jcnowztVsW7tl2I3Zu6FmMHT7gvO2V1sHO6RFeF5n3saUG75fbY6LGrfPbpBtRJQ9YMqQuU1wxMpSNa4AIfBmJxtOIfjUTt6CqJyRXPl%2BLhNtYifgMtSkY2z49XgHEc8z2qDJSyzSd4TBWuqQu2hiZVQx1L0e5aoSdwghzxXjlEqWXmj%2FNfxA4rhyKknmpCjquAWTs3R1aIui%2Bu5yvENh74bTDNRIl8a1FiWega5wuuc0OjweLjoEa1cYL0c02paL9NgvNEuRD1B%2BNpGZAXydNN8NOzEdKNa202HlKS33i4NjwAo8W3J7VdQeWzvaGnrQtuyP4c%2FwTonbFYnJjkeXYecg4%2FEd4%2F52llG6wdzEiYaP9jsqeO9fm0dKLt%2BRmgKJSAhOsiJGYfFk%2B1Euj1p4W3RUddlBnqUoxb1VEnaj%2FZu5mmSVgYMSmnWnOBSYtDi%2F3kwru%2FGxUtK%2BZWH1rvvBvaMtMMz%2BvcYGOqUBqWBcPHGmdbHsEkBchDqTLEpMPfUDLxe52Be27uv3m1vKa5HSBvJXDUZ7GbJ7OKyB%2BShl3qE%2By3VAJYRXLQtHNDn%2BSuairgJc5nNBfF712%2Fn1B73b37gUAriSgBLOlEQKSgX38ErD2D1ufBNRq5Lx7ugMPAs8Hn7gjoIX7Md1A2pZjq%2F6nNckgFoj93tGT0a5beYEEnNMNST%2BW7oc9DSV1pXJCw94&X-Amz-Signature=14087349d196b47ad9118bbc6204c48b5a1b627d39734afd13be2fe21755ab32&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

