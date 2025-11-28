---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZEF32HUH%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T150051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHNloZ6LJT0hQ6r7XJcfEVvMg%2BkABehCTdQRI6ng1QIaAiEAg1QqAwQdcZAe6B3nVkIal0zVy%2FHpxdmFoZcL60jMgr4qiAQItv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEvbdxvEYZvdQO04WSrcA%2FPoH0DH8EFdYAvqbiJvMwincBWV87Jrue35tY7cMb5Dr%2FuqIYKdtClOo%2FAEO07LW3UenpaA2jPGi0jImnRlplh0s9MHvc6zjrvU9%2BUMid%2B70EZ8ewSGv6TtduxKtmQTTKcLnTPNM0nhFKHFx%2B6bau%2FQAekPpRkZTYQSa0du%2F1euRzwfjsiOjprOElEDd95VVawL9OXki%2FoH1T21qokAA9UDWyL8QnshzUW5skuM%2FjGG8p2%2Fy41jotYkOOtmNjl4e6NoQrGc6BR0b6ei30qhELeLKQKC7lYj6ucolh8E1dGEsW5uE7yM%2BTHBfhHnh6ddyISDsALFIma%2FSU%2FZJJ3SnCNPpBgpebfI5zMSoxYPlunRKekAAalzR4CdcAU85aiOGxVkYvnp71EsI%2BtX%2B20VpT57kOEKIWM12BpXe9ahKZF1xeuFHyUM%2BEhWCMoAiev1yU0EFShsMiNTTasqFnFXRQPZ45%2FwvJVAtXcyYnuvrzmx8yZJOVQFYWdu5sEdeaUBXzKSeEbDHaPUsaBmJErA9VeiPX%2FtYvBh00mUpTox9W1WVfHQw8UmquJd%2BI0QVY%2BLv2rc3boCdVUXgsINkV2OOzK%2BCF600XSgcKvrZsbMGxTAVDMw3ZrYIFc2he%2BbMOO%2BpskGOqUB18SPAs%2BNab%2B8c2Fla4XkuJldz6dViLBG2QpH0Kwy1S749nJ2VuM8sZ6pQEEDye%2Fwpf40%2FyM7ARtKqGS3xtz6R%2BmSygoZsrLAx%2F9e7QKU9IDkhGjj96WMzE5AlS8M78I8rjzDmHWampbQQU%2FjhcBTcwjJxQKYCvKtdxfaLMK7Nz1OWHW7YHuLmax5fEhBWbbiDZDvdSqCfKI7teN%2FaeZS4OgP%2Bl%2Bu&X-Amz-Signature=933d4435726a53584a38c52b4df9af04212f6eeba136a71d9b55d6e81b26591c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

