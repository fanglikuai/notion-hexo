---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QUZBI75S%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T020051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIH5fwqnEgSPaQpX2bmO7G2CuNHTALKy4gYNC4%2BUexK5UAiEApnSerlQjLkanzSxe7Z7Oz1W2yOmU8Q17J%2F3hD9GEFtQq%2FwMIfxAAGgw2Mzc0MjMxODM4MDUiDP%2FamuoBrmdYD94tkSrcA21D0mUWe7sT4VYPh0ymZ51PLH7WCtB8J2pAp%2F2mKmx0YtlPAtwSVCZ5zyIyldVKEYgugS0fgOPUUC9QonSUi8SnFy7%2BzpsDXevf5md7vydeOGfaZZfRupaiMQjH5Pd%2BYqxEYh2HePPQgGF3jiFqpBboyr4hJbMDKrs1PAkhLGXY84wkAy65ZZyP%2BnZrjom86C0ZoeJSEOepHS%2FnNbCnw4w%2FgjE1moEb4y2LoOaSdkVEmADF0ffsUM1oF%2B7N3C14XkY8V9CeIjd9IFz33LOnNQ7Bcv%2BqCbTRUZtwzImhbAcJh%2FSHWeTM3uvhHxaPSumS8%2FmcXUtPoIUZNU5WEQSs%2F4apeVAWD3Wfpr%2FYZ7LoNmNzBqt%2Ba8w0tbn97WNHVEtPk7565ljODA1VO6rG2Zlv5%2BdUKFiHFcy7smUEk8kdkgGlFd2ZsQcgDUznc34KsX8k37%2FpSVA553m0nkmSS5FFyMgASM62r8jHDIyrsgFuDjv2DcUHk2WlKFK4epRLvhK1mBGKBMzqXVnOsTZMq4Yb7KtlRh6aP%2FDFS0Bxp%2BFNcMKLXReb%2BrxCFARcYazJdjieEJhyRamms81efXbwNV5foN5B1p20u1mzfcG7GomDe%2FY8M4MgNsyd%2B7jyyM3kMOjnqcgGOqUB096di2v2EFVe5hx%2B5UCpc3BqNzwq7h96IwIOE7bWQxwEXeuptEYkhYY9uIhQtdRbOWD%2FgTM3LLurcL%2F4KsFowi0%2F4sggQYKFyrLIRQK0BUS52cc9LFR%2BW9mfEpOLINOUQSpSiWpiOk3d2hEw%2BB7BeXXtKn5h74D8Nkt2j3ejV1i0ROQNcSfQ7NxCGMOsPf4FzC%2B5tEAW6TZ0QN4y0BZmM73XwbXx&X-Amz-Signature=a08f3c4295c733153aa5c9f42c38a1603dcb2b6e30ec702bba53f4ee1fc5ff3e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

