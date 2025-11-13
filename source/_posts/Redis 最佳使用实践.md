---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R3RN7SFJ%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T180046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCID%2Fh2s6UhLiZqFhxsc1X8nlBqD54jBZ0oroch2w3ZE%2BrAiEAq0DZCF8Mw0RxogtAwCTuBglfp%2FRGdsM%2BlogGsXa1mjcq%2FwMIUxAAGgw2Mzc0MjMxODM4MDUiDDTIfoUnflaao2F0zSrcA9WnP3JkkFAJpXv4xkSrsotMB4PJ5X%2BLHdRgp14HiwIKbEJWyBTMWZrNW3qUtH0dU%2FYiIUPcVdC6acCd%2FWSjJknwx41tLBHHrrDnG9hciR8kZd6ghVMHRYXPOMdl%2By1x1jIZXFce5ZMGNUQ0sqHslDmA59xyQX3RK1ux6HX1eRocgSceGLYBfrdGAGk8YF5RqQWSQyyCfXZVZl3XpBnz3T3hLa%2FOHpEGIkPZEPCUqhAEfZJymZZA4Mr%2FzIHvU9tZQUyBdrsOJAT%2Fx0tZ7E%2FNjWmPoI9FFoGL92hxynXVGzD1qc7Cy20p16%2FW5j%2FwS52f%2FGzFlD2O733aou9a4GJ1wiBId%2BYVa%2FiDkdhBoaDGm0%2BpLB%2B9KckaVXxx1VYgMWW77vUDs1G2vRZ0vtS7Ck4kYJBj4odSZFTowcRRVe8a9TStBh2qvThd2aP8Z7nfgcJSi6pWPWb9Pb%2BMgYgtldBgEJ0lk2l7qB8Mejjhn1EAnm5o2W2rTZR2JD2eNvR5vLOVh3tWSyfPyIihJmRQu%2FwtlhHdLADbVcuYJKu8iaNd4BKQuWlVnh6ZmJQRn03s6mS2R%2FmBbAIJhM06A%2F4JUyOxTfJ1JIL6fZSRNUANzr3E5UXWUE5L%2FM3D6SD5sieLMOCw2MgGOqUBQC6y66u%2BCehS%2FfjUR9RQ%2FHEefi7CdFLcZwTeXhXefs7tptoolpA4v7iDiY6lR%2BBXmGMpGnk%2B3mE3K6uVYMlxV44UOFaCYxRK2a2gKl3sXR8uQ8OeJczhs7RGVFhE1WZbs%2BgucHaXr7JNoE%2FpNjugBV6tG%2B9Wn9MkaOSyj0p%2FjygN%2FehnN9zTArlv8jaR7BnitrEiqNjQ4y3wtUAWcH2hLSR7jTxS&X-Amz-Signature=e3166488dc7803380b62450ccfbe45cbe116b22f46deb97b0e062d466aa7599f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

