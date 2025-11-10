---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WBZXKWVM%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T100048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDkaCXVzLXdlc3QtMiJHMEUCIQDon5dhACEcJyvCIIZUNJGMZohVqOC%2FUlQ0YTHC84Wf1QIgZc%2FxT0Uxf7ZLM1w9EtN3q7CF%2FShSy7oXA8ABxOo08agq%2FwMIAhAAGgw2Mzc0MjMxODM4MDUiDHqaHsID5EEl8%2B4hxircA3ZJHQ5QVox7maAIKNG%2BxIZqK18NbCwVVxdUvzwpvK6q%2FCLHFbBn6z2llAelddFoN6rLk47YEQGitU1GfMHyxIrnHrHnNAnVqo2SfbFm%2F1O614pgT5g%2FoqwGjSkt2U2drAUWOSAB%2BxeFAQhKCZ%2FU%2F8eUvQyxzM%2Bk5nKqaYiixOZ0ystuqBBivkNZqB698pJM436U0mrIn9OGxhyLQCSvcK4GCT9VZJHEAGbNAKC5SrEEDJfjxYd%2BntTCrasPbCQh7O3oa1B9jPcaxVTsVDB0pmutoTkm2kGJ45mSJkOfNdrhBBN8ZshA8A40vFngpm8woViJpZORUfS2InZ%2FVA6T8ITu1umuhmOKJNukUKL4y1yXmGrlz%2FBFWr9c5iNqgkT5s0DPaytxsH%2ByvTPkzIT%2BApE2vTDebntFL%2Bbr8uUgt7M1qrrz284IqMFTNEelHJG2dFfSbrGKBDKE6UbHq%2F%2BX9XHWSdZthrK3DGYwe8vIeq7YT7nXURU%2BWBRe%2FL5NF9bdbmS%2BR%2FDauFFFtY5WEEDJC86QMO0WDDR8xUbAeiiFGr2NBW2j3PEf5red%2FJb3N1NpmoRNkpYKheaLkgOtXmlKdhqaQe0i5%2FaGETyz0Ah5bYg%2Fft9Zykmnj8SRd1xbMMbVxsgGOqUBxskOxoO8xK7KK8139zhWRrQTsswxmJDfYJjFCwzc0oz60Il1YPI8Dv6ZCa8H9l8xmyyrvN%2FNCT2nprQ3m32wbj7WXP5XeaNh2DSn%2Bjn6ISjbjHlYSETNXSoK0awIGnabgC4Nt3ADzUK%2FV3J7zYFhX17RlZ4oS3Yrt71fwD9kdvvk57NXlTQ098%2FucL%2BRYl5vFnSbK2x%2BREMiZXQFnFWpLj%2BoyB5y&X-Amz-Signature=aa82bc0bc93fbae25d0fef1c5c9c7a524bf40796ce7742090be4035bbcfefe41&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

