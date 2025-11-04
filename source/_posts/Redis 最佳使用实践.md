---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WHOIBM44%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T000044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIG3uyc0xtU%2BVIj3nxNYDv7SyXL%2F8WhGNVcaAIRRbR3wxAiEA0VwdtdDjrekx5j6FQVklVUmz6H8b7P32qRcz2NUWvz4q%2FwMIaRAAGgw2Mzc0MjMxODM4MDUiDK%2FK2XGMUVcVmOoSdSrcAwIPiN3%2Bunhskp9Rr%2BwaMIQJRkQS7WnZ4hb01uPJQjh%2Bf%2B%2Fs4x0OTbJsyBDTpbYnLPaGDvL8W3SAY6tIXw1tYxaA2XabyiVtAmRAoQYTtPxSLsWc7Hh44BIrCfp8NC%2FVcSbqyUQ0TQejsIGRGbC%2FSB2Pm2DhnftVo5wb4t%2FX90ejxe8rZEVD5tf9%2BPx7aSFERPCphHMqDo4pkgyd%2Fd6VTB4%2B9FFS4bV0Dc3krCPj5aMgSdNUSjej%2Bj6SutiwR8D2GyRQIBkhlpiVYOAI7esT8PyZ0vsDI7Gx%2BUxwSxO5yIFGiL%2Bj6F2eKwVqs1qCIECYzssfHGUy0Nfc6nlGcGUF1oE9mzrUkeiisKPXMeBPX%2FJWhA2AkTjvbztGa38SQ%2F2wMh8gqaTdrUip3GtUavQbqIXm4b1x01WVX8cv5t9VI0318rJJBz0vW58d9Bjx1BenycaYXrS2NEbowk7owL6UdP2S6S9F0lssRHmQ%2FNpfVNJEWzcv9XRdiajUxmjL8nA2kw0rrpGDLk9J%2FcynvdpaP6MRklKwv9Icxr515RhlHwGxfQa9TXnzwLMttuxBnAi6SWTkMkObOHrN3v5W8ChvNw6nMcdAzmafTY4T%2FerKjJAIVWBy8Cykw0aFAINZMOv8pMgGOqUBUAWVcuBJlZH83BSJ5O9lt8Q7yYCN9MV3DQT9p1jzq5dNKme7WMO%2BdiCmSRsQwJMK2eul4S%2FfanhT%2FIYAo8jWJMGzqz9wm3zGDqzUzBmZDq8lGx9lNJD2RADQwGPRy9it%2FElzCRM6oTHcqJxfFbYPbB%2B7W5K0cw%2B7%2B4tPWassl%2FP3l%2FYGVDSeuX3TjS01GEMhsutPzYdgVjmzsVh0GUdgtDESQJx6&X-Amz-Signature=b294d5bd307f5ba93f167b2b68d475d5fc20bf8125159857ec9a80cddb623ef3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

