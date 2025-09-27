---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666TFG4LAJ%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T070042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBUaCXVzLXdlc3QtMiJHMEUCIB0J1JyBk1yT1h8DUc921bjBxnMsLcdhLPR4QOnywYVAAiEA5WjW3S2OWsVwP6PAXpZB53LJHDNjkwmg7gNHRdK6qWIqiAQInv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBG7xPE573zwhIk3OircAxDrjqpos%2Fz9rlFM%2FDGv4R38W4VXdVuCfXhLIt%2FGg8GbTLEl%2B2FP8lsJ3cvL2NqNRU0z%2FJOdagD%2BZ8c%2Bgo07DXrsUXWvflBstVQ0dG6Ljb1jCSXVsPkffCrvkkmWmTgEqoypIK%2FixOgp27VrWVRr5oGDC9xUFNJ3EMndRVOLC25VYQyCEv4lptricSjfekrpp7100tRF1I8Gi0QpbnO1emSkDSeKqiBq3yj%2B38e6ilQlSRSmo6jtPTOhJNksSTvwxx2Y5YcWPInq2mZXx1DtMdgX68lxaPZetU1yEswYTf%2F5PfGKiNYUEQ6%2F8%2Br0JXezvLMAPINciikTADDvpNVnjA32vDs%2F8MAq01%2BLDt%2F9h%2F9vcU99Pw2mNNRP8%2B%2FBMaZRJJwBUfNIErxr4ifJhDhKarh1422eoQbJYBcq6gl694%2FLKwBsPCxV3i7qQcn7IZAMrpaymjWVArXLcMkT53PXvZTA8keaDp58HzvMdNbFQ4KaykM93AFBUcuxqrK7tO0l2lC2JSjfXO%2BMFEXrJATshoL1xV%2FNbAOsWIfavqMXekRwUbPtYOrV4jvkAqe9DU83RQ5gxPfbQ0y8Ubojqn7Jvr%2Fk9uqYADaUAWmFwnqC%2BDJm7yN2TCGlfY7me60WMLHc3cYGOqUBWjLVvShKcfGtDWBMIGG7s7LhNkYWybb0nojnnDd8ziWbYENtPou1nJnijOWf%2FnG3%2B%2FyPVGfwZ%2Fv6oAq92JyF2qAfRCqJQzpW9AWkK8UtolDnyIb6uUUIJXbqebaKRT5WAbhnZHcnuUX9G6tCg8yLphgEL4w1N6LA0GPDlnO%2BDy%2FowuZm3TGFLp40eCMg3usOq5Yco8LG5QuP0PZqYpcf4JbrZcpl&X-Amz-Signature=5df10da64f6a2efc0488421fcc7d291241711825130f6c8e3e54ace6257657c0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

