---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SOQWMDDG%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T020052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJIMEYCIQCQGZYemQTgo7AUw69HhRe7TQzY8rlvV9sqzK3oN0wcbQIhAO6m7yN8FHbd%2BqfQc3eMM7OSjsdV%2F68RqerYv%2BDxgcg5KogECPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyJw2sl1n2cdriZ5MAq3APH9TRyVqGqk1zjER6r0MuuAh6KJTJ9gXTDFCr6GLg%2Bn5X%2FyJIzzJ%2FaRuMnSA%2FqJMhtGXiSW8Edj42tKrR3gw1k24x6K5nt3AMiayQ58mjWXXHsU3BgRLQrbybFgNEIbDsbrDOxmROaohaw%2B4wQsyqpO0PzvPKPy9CV204q0GECgKuiyLKiNg1d71NcdKV%2FdYf9bYyUWo6KEdpBYIR8qkytzhWVaY%2BdzRkoqg03REl8eIR0g84BzrGlJdgv0X5D7nauGL6YPhuQ4C2pC1TNTz%2BKjWlng7PciON9iS1j1N%2FSLi3bIUnz53GkzGS76j5WZbkw46hOeq0ugHrzYyFwyKjpcl3X%2FknNCmjS7YRICfcSgm7Dku%2BqOwF40%2FhzgRVEktNzI7iTu8MWHlUEC1E0YGtZKCgKL4ZOCbyMhryXN9FfqcsRYj%2BwsDLqcgEx1rm0M21dnS7wKkfZPX4Sp7bJfZH4%2BSg2l%2BJLWcxYd5XfdwieronHnhCSvZr6rvh%2BU7IQu7F3rRxG%2B5xuG1RpRJA5WmkIlkoHP%2F1KL4tYgPgKQY4Pjxf5Z7NI5v9gRc%2BGTpyvWXJ7Z0qzo4lMNvtuztroyD6mjZGG3QUs4Kqod9q5GrKpxRt3WoBRTSiKOz2ebzCu7sTIBjqkAdXrE%2FmHVYKF6qeXZA6DLIa%2BnkQgKNXhQofnAQNAHocvsvB2Uahf%2B6digqAlS7xbBAoGuygJzb0wIYoS0OATTRpkaYSPkehUKIA1WbiErt%2BTEHAJZY%2F6EuXUxZttAiZJScb95Yt9k1oT7m0XYpf0dYsVuBW0hk5HyMvTe7WgGdHalaD1ZXILYZal%2BZI9LkD2OlJiV4%2Fla1LaKNSm4ZFlqgsEzJcv&X-Amz-Signature=be87fbc46b16370e145edf5d6f744d86fb98d22cfb63789858822e6c09d52125&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

