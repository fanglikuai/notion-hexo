---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YRWGXZ2X%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T070052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaCXVzLXdlc3QtMiJHMEUCIQD2QQi2XSipIlqPOV4rKgg%2F6uQDpkpkC67nitsiVYj%2B4AIgbFOdTgxFJJpPsAhegDfRJO%2F1liRuoTo7682WQiFVvrYqiAQI0P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOsrp303ImD2VLnnoSrcA0qe7y5GBjSzwODo7RhaB0%2BJ%2BQlSgMPObk5Yjhcqm8I2mUFVPqyFE5E9NTqOEfVWxF1WUDmuH%2FTyx3Y3uilBURzZxsSN30hekJXm6hY6LaAhBR78bIM3juymxHqrmE6%2FkVZ06%2FtUPgc285fnDvc9I7xBUBBqUo7FYVz%2FH6DgZ58JgpZVVK6JFUzmR%2FUrL3SVK0BzaJOlaqCr1LCA3Cc%2FF%2FX46yU%2By2sKGasrgANhnW0Og59kB5P%2Fdlcb7Rc77f3NmXdWbhWfAzzeHXfcZZPzCIwS5aCQ9F5D2vCGHghPpCcn%2FqnNdyHaiwqD%2FaqEHQeQIv4kU8D8HA4wZFnTRplHDmEEvdR9RmfO84BBIITOrUM1bDfW%2FJsjmpJThv5wqxsbi%2BnABhkMLIaV3k4AKrey9R9i%2BwbajM%2FMnIJ%2BNd7JSguPMNYIelRZXg94mrz9mC2%2FwV7wBoLQq6RMFdHLNuJ4hNK6vkK5PaFM%2BdDzqtcNPEiNWsSUhuxQLP%2Bk%2FQoC5VXy5Jsv%2FRnxKSdFn8ZxGq%2Bri%2BfJeIWXQYJtGMwCLdtFoGJMBlVLhnl3WgsG4eAnzZh8QNxtF4kHfEAm97dfg8UvPhTq2c57f1gNdTVFfJHO6DSZSSULSug5uRY7%2FKhEMPeN0scGOqUB2ISPLraW00SWbYx%2BGf%2B3a%2BszyhLaU492BA0aQpz%2Fq3eHgfjnxBWmIwlY2SpbGz6BBV2LuWsCP8ihAH73ypW%2FrbuKFMoxzPlEHP%2F0GLVhQ1lSxh03mSErV%2FrC1UgtHKwT29ZcKggh53w0N%2BuRqdwxn92hY1rJbdexHvxNO3UDrcv%2BPHGQsAFcn1ebc5kANABh9SHwGMPwQXAy0jAI3pCVr707UZgQ&X-Amz-Signature=80896d6def61fb30c1cecc80b175d427d9e7267ca21c0f4ae9aea37e6d0af39f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

