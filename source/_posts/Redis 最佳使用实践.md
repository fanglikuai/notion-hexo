---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XBQKXZFE%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T050050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICIJOhYeLS9GbDJtYTbjvCpO2mXNpuDs7qFAehYCJq3%2FAiArVh0ps3CZ69bn87Xx5IYhXrtfPbvFxofPwH%2FAEqaa9Sr%2FAwhWEAAaDDYzNzQyMzE4MzgwNSIMrmqvjfFb7EbRIDLwKtwD00Zap3tvGCipHMkYq%2B3roC2RX7JED9Ogt2J%2F0i7Hh4kAFojb1PcBnC%2Fz6Gs%2BAm0obuDEqpz%2B6hFOQKMNAWvcoe5QM5PYJGpNG6GfloCrLok1%2Bo8yX7vnRLbDFqW0yEgUSO%2Bw5OFCxXDLfaacwPFXY663HUa5Sa83OWtXBgXW6Qaqb%2FjBLQRUNVTVztBnlUPGDs1PSN3TLM8qxalYLeW6Hnl8b%2BEIRE2DahEJrAnCy6atxP6RYJ1QfpBAYsN5yR%2BwHZ5xvCCmz4%2BjDMKBepHuX8Qx0jAVVDLODZT3%2BIqJTtkAmTNa320c43jheBFrBhHhtMq5Ds7hG%2Be3MXLRf3Pc5J2Pfo39a9e3zBVhwJDNEX714%2Fo5fKFTVAsjiOmu1OTXAaD8MVapOcL1hz%2FSLJd0dahUTJNiwKyRgYe3aIpFn5zy44X334ZAOXhiTkms88qfz%2FgQ0eEufy1ioAP8dsNRudiGdArObl%2BJgFJXjtqANpKE%2BrDMFjPPpeKBM1ID2Oq1E3QUV2w49upI1S9ktZH7DiCFiwfSJMvjSYAhSO871N79O0kQhtdNMHGLW6iC%2BdegU1dDTJAMwclmRa28301ZgJ%2Fx5XBToB5O9Ih186bgMUzF0iPiitMd2aj1tFkw7%2BnNxgY6pgFEVX8hCBLoRTASCDgWP7qujIjkIMaA6Kj0uHL6jSXM3Z1PHWu7WeTRAoh0mj8DLWBc8xWYdhAejSXK%2BYTBA3pbNZmmukMXQhDMfzo4dOCv8sgINwSafKBqDbtM4z2ArELgxlVsXvr0ZoDKQ1%2FwELl9ltIDjEn0M4KZD%2FaB%2BqFJLsHUqoGS8g3zwishR8bWb6WmnvsdWseeAojC%2BFeLDAikSejmBq%2BF&X-Amz-Signature=727690b7947b6a85b28433d96a769edaec52fe90547019a9313f1a9837856e45&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

