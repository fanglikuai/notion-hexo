---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VC4J4R44%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T080051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECgaCXVzLXdlc3QtMiJHMEUCIQC%2BU%2BuoAhejYhQ1NLUl2ydVg0ZjLPL42e2RDB8865ysswIgPx7Ltrtmyn4BwH23TO39s4TV1RQYcIgxLfuZttHJVI8qiAQI8f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKwOq2VtZlxU9WCw7SrcA3Dczt7oCkq5aSRON1nX8yhv18XrFNrEydO3FPgQZjTFPeybBH0yFyLdOI%2BZAYumleBZAwfbUS84GLjxnNNe6IFvsyP6ayj3YUjLTY8ExyojsXSZaUo732N1iIU2Tkpr8ku1cBvADLsJShpvfwtClpV4M2J8EAOJ6DNuWltM6luNMhk4Ke%2BeSIe4RIK7Jg7uuUiuoTbpJs%2FauzQj0n81TE0WwVlTyyPlMhgzC%2F%2FZ2fyEvRbiRI1ADHr7hAQrCOsI55vOxrvE4MF7nXEBI2NcgXPd%2Ft9HjahVkU9u9cauotJ%2Ftr1%2BH7PZ0G%2FNJoLHdeMRL68sb0NqH8McHW%2BArtF%2BwwgZPCRd39vUJ1uAANiPEio52%2B%2B1uySWIrb%2FYwCT1G7oZzpXTLwdznu%2Fc7c5KW8WQlgUfIDxr97j3zjqx8XkbtWgnxxI5BbQVjigkpMZEg9ugDL60tuH4ygwPEX6X5jBxeuJnCpKDNaej3bGEKjvBHVaiAyfUA6XS9K75Hvjxln4qh2bvni%2BXb4a4fFKpxh15JVuXoVyBYVJz0Tqr%2B%2BkSkVVbwwbuylQjwrOJA9y9O60QrTYballsk%2Bzfk9dtDZLQm%2Bghs6Dwt7MnE%2BxqCPgO7TvtOrhfJ9CgH42Q6WRMPeS%2B8gGOqUB9lN9wO6VBrCudpzN3cPB%2FzMmkfW4CTnpG%2Ft33XWZeAh55O4aWRNiP76nuKDB%2FU99QGkrTC6XjnAbRwTuSxYfVDKgPCxnl3c%2FUm23GsoASl6oPehR30YGm%2B7wwXMusEtKOblBk4J2Bul041ga%2BTwqAaiyodEV9QBvfkhc%2FjSOkM8NJv9Fw8224uKeUp3w0Mq%2BYcZ%2FjwFW9sZBtGD0tvEn%2FrzT1BTk&X-Amz-Signature=03f31a0429af617dc22f44989036b355b65c52f4ea5f531dce181181bdea26f9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

