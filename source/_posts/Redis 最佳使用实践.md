---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WG3XXE2S%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T140050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBGgLHWzYAICqDFX%2FewYprQg%2FjamiyVPiF82YlLlHGP2AiBEeS0YPfjFb0wwEo1Igm%2BAOTEjxcv0QlQMKXc6EgYxpSqIBAjF%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM0h%2F3pSw8JwiBzdvLKtwDkFgXXm%2Bf%2FchblOn0TD%2FckxGdFkfz9ieNhu13LSs5JRI5iHsAzQNnUPfrs683ZzoDXe9q0eePwq6syTO8Ur%2BiZbRjTAvW%2FCM8CSD6uGk4ux06cBhow%2BzlRSaH6atiJgRG%2F59ZhlyBg3D%2B5uEqGIxxuxV63d6ZFJsaZ9CWaGmVnbs4NIJf%2B4PVH0pGi3uhTIV%2FSH2ZXqdlJtuNkmyK5tHLLw7jlHlsJFHAXpCgv1SNrBlXiPmRcVLhNekx6aSILeePYEGLlpMKk4ivPrR19OOndi%2FOLKBjdT1UqxBrhomqidfoiwYZm4ELTBnYZthv4FeqUVH3BfSUa2hOymEHRNSWTIQRyrgvjRJfR4qU8YJBCfqWzSsGDD5J3p88AOxz42PXSULq5tEVQPp4BS4CpiRGeavs0yg6WPQq9jt%2Fdh7jwVgQNohDJcpvYKk4DWA%2BTUBFxuihb7PuPUyCpixqSJMURAw2t2ppkoSZS7G0Uxlv2BFipB5UNcUWR9u5FxZrY9KLZ95tto0iUGCq53XEZUjdGUO9IxxZt9Jf1fSbflBcFFNesyHHkBzQlh1GU73Si0sDQStMYXGCdl5VL42OGvj7mFE21DdcdBKX5KO%2FbPjgKwsG9CrnMT9MwJzLokcw%2BsTxyAY6pgFyibE3vxdRjaUa6P%2B1%2FDajMnRBKAQbyuGKd8EVdhEAkpOHVeqh5bB3YCPPeH1fsttGpfhbflAKRgADqWGkyWS%2FLyQ1wx%2FeqXLeTVK%2FwsXlVCeUaOwqJMBxUEwTCamOt2oNBZg8b7PrzQWaX%2FY13%2BJ7rjGW6JFZqk4BT%2B7v8mV0PLjBokTR1MI1jpI4eZCUUc3suHijiHiv6uABwqc1LEoT6v4zjBTf&X-Amz-Signature=cce602ba1a5cc2ec16785ba8cf38bc3dac9270286ec706a6d9d26a1fc2442889&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

