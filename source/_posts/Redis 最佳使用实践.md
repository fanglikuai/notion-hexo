---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666VMWVIAT%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T000037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAAaCXVzLXdlc3QtMiJIMEYCIQDMmSTrLTPg7XR6NG3NEIPWTbX5YM7RXS7zMMp9%2BVy1XgIhANw3eUTV9%2BGHVt%2FzqiC0fjCvErXYlWJ4h7c1oC0fhEicKogECMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxkFcudclag1FBfSw4q3AMOT6LUrEl%2BbuMOrPhQ0x0jNmAher0cgdwedY9v78Ly3a3wseSeQFHt1Y%2F20yGNPG19jC32xe2143%2B%2F7UptqyO0sXqBkcSRkKy6Wh%2FHLfedEvByptt5HJeooxpMcydvpUQB8C%2BVfLkRh%2F1DGqhvgP4mV%2FJc9yuB20dngm5iF7FwK7jb%2B9%2B30OIitBm4L0XJJEfTtl2Yrd2smfTwPiziM7t2qT5cWruwzT8Pfe0uoC3z1CEwhkZyFcih8JPwdSejDPQc%2B5aWAjKRXSvD2rk8Z8qeczZCOAxfkFOheUw5eXonp%2FCrY1O1VqaQo5L%2BnNC7wHS29KRBx6tK3j3Z02RKi%2FFK3LvYsNF5hypRG1QhMZE4KKj3iVlV8GT%2Fyw3j0es67DeB1Tknafr9A1lcsUOfQEMIpbdc4%2BHlNGQoaCU%2FsFO94JMdESn98xAvvqk2pGStCp0BI568T4vuvHi%2FnSs9rkfQbxEPXDIDDlI8B8Z0Csg76RRWNq55qhv3HMMW2qfyzh1o2QB0rCCuPwuf7%2BGvBpOBfuuIY1PMCPe%2BVHwikzmP3mdavYgVJTruZr8zDg5Ye5cDds5DkGI9ZT5UA0F8y6t1AUzj%2F7AfSPSVmf5FVOP56BCnFjXYxCMibiO0CzDp%2FbnIBjqkAa5g4P654ratxWS5cgWy75pfNxhtnDW8ZUc0IFz8GyxW6k1O8FxLQggig9TxZ%2F9ljpt7GcnWTnmtoQbz4WcUshXfkLXieH1rtfqk42J0piTMZZwZteHJH7YGhoB%2Fbz6hrnq0l6sMuoZYnFAq34RZT%2Fgg3QEW2mzxQGfQ63ink72EdICx%2B8qVyT1YdXFJUNNG5Tsta4YTjIt%2FgAQh3BXhwdVje%2BxX&X-Amz-Signature=aeab537f15424a5021387a71bc1751b6353faf3ede6303d69c885e213e2cfd1b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

