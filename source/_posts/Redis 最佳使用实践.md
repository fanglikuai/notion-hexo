---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664XNIOGIF%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T210039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC7sCXU%2F7MzDiR0bMrWs5rJCr57gTFCfDudCVB1Q5zYJQIhANpE0%2BJp0H%2BWcMWeWuU8eybSJszdRgERHSYmOndf4QfYKogECI7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzmnKCnRWTJ%2BD%2BRJBkq3ANvaCSh90VqN8GfXNmpMQvsv7dHqESJBy5iGILMsx6SYsvqRXCVQwhFRDVoVHI6jMiVlVPM%2BMR7jRTPGD9STGTF9fAjA5I3sCE6WPxpr8YvN%2FKpHzKrT5Jt6H8vj023i5qHoS%2BePsHt9YDFxQhkG2EYLXvEiRHLxvcxDYirFzA%2F7xuZOKMRAv6zpesuUOB5njwNY0a1E1OwqkVu%2F0y7tS%2FOg4cyDPZ1vMpwrghRTD9%2Bsg9Q0HfgfdDtkfD7o7dIiclspr%2BIjuXUPvjLN266uTa9E8bq4I5QqZ0OcUMOkwoV%2FfozC%2FN7IDv57cuhd%2BcBSEwb7G47Oer%2BqD3JgZGH9Hr7M1kPbuIwThPCf3uM6NcbaGB5%2BcEuLN3XE5IQvBl%2FuKX%2FKJDjIP%2Bw6981RXzPITPMDABkOoPfj0JbLvPQGrDNaxbH3U%2BzF1y1DFnvW9ag32suhnzPbnGpbKtPc1qcnWikyOgIKQJOsm1p5iXcE7OinKlyJLe2QiDjQr%2FCscChrng5%2BETfJwrms39vuaY%2B7pIOQNeYAC%2Fm45oswWT0RFQilo9i%2FqiopK%2Bv%2FblZVUX0zGZMEC%2BjJDkTqS5VNtzFZbEA9uIPCx71NODq8%2B2GClHxY09Sgvr02MZQmBfNDjCjxZ3JBjqkAdh0YC%2F3QGeAszjO5sSA%2FYynxlMVQW7PmZNYODs%2B2strsW52l%2FC1b3I5lGL7SEeoCDfRlSlQoHpEOvw3KzEdx2M1fK9aaRqAtBnWDUEr2gecsp9cBUoRuGUwqKKKGv4aD9fEgOed0k2JkCGVlFMm%2B2AHu4StgadOuD%2BmoJXV70bSk6X97SfSmjUMk8MUhVnDsW9pfvCW929C5A49PBAhY7qrojfU&X-Amz-Signature=6d7e68ba890a4dd0fd77a823a4d5946bf0d68c141599c5078b645d977d789140&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

