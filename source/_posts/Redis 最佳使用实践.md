---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664XUNUB3S%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T080043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCanQY%2Fsp1g9IWdhEm2%2FtpdYDnfBvB3levGoYBTU2odVQIgQiGDz7U4V8u%2F3Kdou5LMvhjSGURb%2BSE74iGU5uUVK1Mq%2FwMIQRAAGgw2Mzc0MjMxODM4MDUiDAe%2BKzFEL8gcWLa0ByrcA5vAQeSIzk22QTjqum%2Fs5t5FoAHluYmEi9urxGq0Kwf6Xv%2Bxlma91XrhZwhLraUOXR5FVSkCXPu7w78I1St8nIrYlnNDqOOkr%2BAlRTImFb6b6iE3ED90dW95vLMy7559Mv1CePAkwcsuA8s7Vjr2rVtwgwSbSxA6r0AuWozQpKylFAEsGKw4dZqbqGuhBoAySHenae8MhDdySyNIy9oBY53pUvmZEbK%2Fm%2BmWsQ27uzxLZaT9dPGLAaso5GDkvIswuQI0O2cKBwifaEeX851xAm0VNhUOylSescxQBeSe9VQlX7qiFkx0Rmy7sIeA4mM1FSkAlFoZwqPFYqlmwp9H7D%2FAGNUCux5JB423iigZKwXnkJHg9Rfh5FtyTWGrVo%2FvjOVGXBRxKpyLcxzjqTmWUdosXVFbFMI%2FoS0NnIvDvwehLC8BfkMVdzqkcyKsCqzyXmbKBPOlzXGvy5xZgg1d7Ii8HNzjpRgVDz908am%2F05EdK8243kO%2FGVXbbaPlUXldKAn8VmS8OAesaMgK85aNzBo7Z9%2F5fKfhSSJg%2F7c4e73TMzD1gNYHbG4mgObh0PjqXsn2RjXFWxcQvicRDNwxR69LXvikt%2FIoSEQ%2Btt%2B%2BnwWteZz6lfAbMNSIw0%2BYMIufycYGOqUB1TjwjISG5iTzNAylGreuyrgRD7qsDjGClLewSoJ5INmqxG7JcnIoUp%2BW7a4Wf3gf171CrUsjCcH1kppRHFKn%2F7gxiQoyy2bSIZg%2F%2B7oUMFnNeOKYj5QuUKxXaQZd730iFHVRv%2FCOkCPIpz52JHW6lXGajTCxVaHKG%2Bo06bWj6%2BJxegqY8pGe%2FqqbM%2BNOGbSdRgokvYymfq8inZ9qqAdm9SHry%2B8I&X-Amz-Signature=1b8200127774a372bcacce39fd58234f308861576688bcdd7075aaafbcfc98ad&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

