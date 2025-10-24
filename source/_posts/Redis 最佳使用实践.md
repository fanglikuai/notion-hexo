---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QMU5JOSG%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T210048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGUIVHpbC0KEbavg3wUgR5edQ5jDnMC%2FS%2F22jULbu4OkAiBT8K923XvmV%2FCVahd5Xk5ZWRqHCQLGcZOUKBVMgXRH3yr%2FAwhlEAAaDDYzNzQyMzE4MzgwNSIME9aBiHrsW5VnTzC7KtwDba28hA4LjD1JLFrGswDeMZ9E5%2BrPqT0b4mko6SgFtTQVbhGoYDgRCfMtGUfA93Pk1CWE2nolHJou1a4LV5vFQRn%2B38slpUDLxY8TP2vPDV76gOWCTFuAqIsC6WaStEW9mdgrbrFlqJJcdorFZC%2B5qNHWct3YQYhBSiIXs33AbaGmmBpA1tO9JISpyIpPb0MUPAr7n21SHn9R%2BGk7f2DNs%2Bu816%2B5iqAqQAgkmF0rsBLKarmEk9UR%2FcU3wT1i4pk69LsIMlqzxEv1rcjqi3Pt7Rw0EgLMfD%2FM%2FSHwmmljakRcMTvfatuKsdZArV7TScdcrmN9FFhfUBg54EUQ98HL1pgoibDC6Jo7PVB59YnzBzOpCEKfQOdnlZqnwMzqj5XE5NXxZrMyxjJpuHGA2BP8RTqHZIUz6O9OkGbu5ECv%2Bujjt48ET8OCUjM8fusp9e0hl0DE2v8v5M0Cdy6JjwlkELNOxXVjnkA1l6JXfs7UHjmc0Cfv7MSoqQ739o9eXowkjTmxuGcdyyL8smpvs8oKAb0dH2Ekih8c%2FcYtwlBJozM%2BGYrcTERs6wR6%2FUNNzgCPylij3o%2FljpwR4N5TMlkJTJ1Ji7hvQqjPJP27QLNL0ASzGpAG5KpiULBpOfowqLjvxwY6pgE7fRAsqRWEo2iSA6CaJ8NDAXsoN%2BKYsuhhxLUhdUPDZelVbh6%2FZ5P2jLMojh4ZHrbszseZ1bF79bPU5%2BgMySDo0dlSf7E9qgQLnLHWwWnv7n7otqEloW8NeJvVJEsQBAEp96yc8bbVG%2FyUmNehcrLMEXQEPqc3pa2MPDPEnWTV9pJ2bneUFPvwJVNvnnw%2Bs3%2FFeLk7Z4keEfQg80AN8J678E5ewvuy&X-Amz-Signature=b38190f9676db347942a15f9aac8eaf5465fa5f83680816584156dcbde005f25&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

