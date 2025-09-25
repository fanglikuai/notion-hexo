---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SZYUYVYK%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T030046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC%2BlVIaLBoQtzeThaYr3JQIHXM0Y0LmgtE9a2FDTQZUvwIhAPfQvS4UcQNYoGd8RhrrNE682vgQemhhO8BZTGQJSopiKv8DCGgQABoMNjM3NDIzMTgzODA1IgyT8KIt%2FtyXGnrSclEq3AMKXCAjEnxatBBMrrWjmcIGh%2BMsH8nZiW1IRtQn%2FSGrNS4nnRe2i2pD3WLg%2BqURRenZl2En2KPycCpbQQ%2F7ebVbTDo%2FSrXl8O0zn34xtvtKSqozx7PlL3KFLKLql3maBt8N%2BXZPkAfCHVT%2BEh8BqHJcNHAYmxiBvVMriBeV6rfHPwG1%2BWn6S2ZwUYskT%2Fl2Jhq8EJNbwLSo7cMMAgUMWiLF%2FT3%2FhYBgAInOuZQq23tgaPJmZGlEPHTjLo6kNY6Dppo3ZMPbyRc%2FaOqtTACRBhggZ5LV61T%2B7rBqYXhJ4xir10CNE0mvmAzuzDw64oK3K1EM3iblugoAPFpuwP3FC9yUoK589QeL6uQp7joQI%2FxzzcDuHLc13b5PyK0nMCrqcPDpf6OuRCrTm9GDH1mXIw%2BxUL3TPxmgUIwK1Kz9U5K4vwiy2tcFXy8nTbxvhRTOrRQf0VBhHlGnX8Ula5ObZgdc4Co0REBCSrV475HXgPzHmREXTkJg60draEz2x3Fq2aIAd9vl8Fx5F6JjuKNzf%2FmLl3RaIQEulwuiv8gUd2AYQSHVZL3KS7IRwBilj3pfuP6seu1zfwLFH%2BbAuuVQqU4wC56YS%2BAn2zWLYK8ClDebQhafTs2d8kkHeuhC3TDG6NHGBjqkAf7M3SSgsmvc7DfP7JD4JLMBUReGx9%2FbrQlMZ14%2FI4r3DzzxQ0X%2BxUoAw6EXRRXU%2FWJfqrGg44ZIGc8S7mqye6nfrZE4x3hkPhuQVRSZuRDfvoeTbi1G%2B2NOEKefMlSa3Apf%2FGRhRvXmHFeUWMSFhwO6hjgtUkcfASl7lEkFjZYECJiBcVi20ATy7HOjWRpQ9XQBGpZ5DSn3IDqNEH4eDWMF7WQ8&X-Amz-Signature=c6dbece46c22ccb8d918c277bcdebc30cdb2517d0962be6f81fd8d2c4ca2b283&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

