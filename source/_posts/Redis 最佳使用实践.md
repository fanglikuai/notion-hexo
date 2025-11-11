---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZDHL7R33%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T160044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFgaCXVzLXdlc3QtMiJGMEQCIHuagCeGVPJmkqJ92hEnHVli3LVGAjiKlyCAxnsRK3OYAiBVSX7l%2B9KkmRiSfaDlWcr8g0DC5zEWxwYkpwWSlOHH5Sr%2FAwggEAAaDDYzNzQyMzE4MzgwNSIMBxELaOmc1HkMjA1VKtwDJRh6RT2fRRBBF0sHBUEE1RdZly66aHwm8KLU2gOMNsmYOzlG1zepbNCHq7qTE5h5ACfxUvCBs2%2FLJ%2BE7gluOfkI4lmnVtfFfEGi9pb73k8SUHrnQZGvJGEKhUIoH8vTbVx3awr4Xnd%2F81m9sQ1OXhfmjdUo2D38ximW5xboYCECO3tDCP2hmyl0UM9mx3wmSS8uqdxpnaQ6KvI077iRIvaN1WVxMNB9sh4W7IUF1O%2BeSt8jRUz7zBKbhTMrxanhwuCXgXQkbkcKmc06V9Y3HNhdOFSz8YVgRtf7QvfM5Vlp0SOxj5R8xjKo2ap3oz3Is2VMD%2B8CFYX7cj%2FLls1EXflKA0S%2FIgztuefKeYmSRQu5idwQgszmJm%2Br8Dpbw6G2wnx7w6lQA53B0RZnlLPadwevfs3%2B0vF%2BO2%2BmzDjb%2BFP12T%2Fy829zfyiQLznR0pZ%2FQ43PeSawhkkvd8qp4FG3sJIUkVbEe2mR3qVW%2FSlpW0IaueCuFUo2vxISies4%2FSO750js854X%2BujJCCuNVm3yyRYNcCIXRiHJHNF032KY49aieEVKkis3K5JIw5p69SEK%2BqfRWLt8A%2BHxD1Siv0sxIqF%2BQlvE8jMog7s9ov1uW6RcaXcgmknyMNM%2BUmmkw16jNyAY6pgF68rHCAjAWpUjnn8z6YVx4a%2FGVhOuwS9jQlh3ws%2Bcmy6MdXBrwD9tPYv6f%2F3%2BtIiCB1wmvEzNQQVgMJEz%2Bd7t9g%2BsH1yI0Cf7kddBrDrMcxC17WIgx8mM7rwqw2aE8o46pM%2Fv%2BxxVN6tdcJZQ9fHFfJkSIM9Eqb7VkJ9VazdMyV6Q9KJDbJOBqPWIUO027fDn4BE0hFf4zqibS5vSJS4ZGe5z6zZO5&X-Amz-Signature=d232ea06deb3ade6c07a7897e0728c3c5ebbf7c1c47302e54de4a9216c56f319&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

