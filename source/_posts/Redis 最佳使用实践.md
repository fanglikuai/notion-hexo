---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XMXZC5WI%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T050044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJIMEYCIQCVTjAPSHLMxsnmnRESPmL3BjlbLPnr3gZicVmn6ZRVnQIhAP3K8K0eHb7GjKT7KjxuKdtxA%2BwgexOyOvm72pIKXVqGKv8DCCMQABoMNjM3NDIzMTgzODA1IgxgdobfKF4bPusJGwcq3AO7ONhKYHM6NHn%2FZskcijQ6hJLc0rWfPnRDSIYPzGwqXjwq%2FIT7JYAaiI1on31Fe0n4EU3wtnHaCI6MeIgP4WOs%2B2FwoR5xpxvLIweGK73h1EKmeclydniU8Lrvzvq1M%2B3ZOnLRPrHGvEcJyvRVMeV0EUAMpvXdI8BGoww2DvZU2qVAGh7Af66JsATOHFcilrzMwbF3RE6jugp6OOWWMX7BOaCJUoVDbXbFbT7ss5psx1Cxg8Fy3JSZTjICcFR6FXo1Ui2qTPpS95dC8m9lJTnB4oDKHI%2F6W6dWsfONZWkXuxdPP62Lonw9uLfdCuzGLlRzTsVA8Ge2FIkFHRmze12L1fqHexFNIvkFwILsA8Y3N%2BDjrRw3Hf8N9xBgrLKg8Kf8k5dA1ISunNfr7OWMnYD8oUgguKhJEONSXA9xLkq2%2BioKYG63cuo58aXFPVH6FkkgIMUjCzG34tsTCrjiScm2L1hXXJ7AldmmfLhJwEZ7htlYaPdZHc0NUC0UW4vzEsmslAp%2B3nPydZ2gfh3CXzrKUEAYXD5F79nBFHBncUoVeZunvfSfPKNYXShadlRtwmoQve5mx5e3hiOpvABSUoAY2Lm3cIg2F3E4e3FD48N9K3KpvrTPEF%2F%2FyUTpsDCl6eDHBjqkAbdnDUQ%2BpuR0Mmo0O%2FNB8Sf3Md7kaHBvfF6mi%2ByoiCrk1J2MBk5XE7lMiYb6VYscZI%2B2SH6P%2FIOojz5sp5fhaXGKWAnjigvC%2F1h8HJaWUiUSl22swgCgfCiQdhHD6bJEbgNmlalGO6JfBZGBsBfw9M6z5CfuIK9%2F7mHd3695SVgYyJncJaEhbH2Ofyy6MzQ%2BZqF6jj90DgTHEDVeLTED%2Bwqc7LB4&X-Amz-Signature=21250e959c81a9e66f2f90c724ee2a2bea916960387d6d1b3287d726a68052ab&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

