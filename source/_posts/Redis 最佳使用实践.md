---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665TXFFI3L%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T030040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC%2Fui%2BKe2uKYFgl9HA%2BRPAj6BL1YRr8ucwgi3kgwB0iIgIgFkeVxBrEufx8axfRkS1IlnP4OVOZnKWsQ0vrjsY0Fccq%2FwMIPBAAGgw2Mzc0MjMxODM4MDUiDLFHUxAXV%2F0goo6b1ircA%2FvnrE1HALh0UnMsuRAxfH5HDDPJNgQ03ORrqTZP%2FaLXR1AWBLFKMjBcRnMwbHRi%2FJkWlct4NliiSK3GvtPA%2Fc7ZfsDl21uovOSrI%2Bju97q36VjKCOKjm86L%2FHpcL37%2FNImJrMOGe7iuB9nHxv0k4IxVQMmD82XIg4ei8qqF1rxp3ZJ7HBgjgZGrv2oTJgSdtoir0Ebk59J60GWT%2FyAKZ1rclsWQjmHftDI6PyNo5u%2FjLd22hMbFv8wTHAecvL9bppXl7i312UtXfhnQJbdj3b0EYDkFTz5F1U5EQr21KRAGTtUu%2FEzKByktNi5FaHuGZnV%2BkIHehcVzZv%2BrDL%2BaNfabH9AKvESUYUZDjHmONAlFrenxTGs0ZoJawJ%2FG%2BzPpHgz2XotZodKQm9Cyb3uyiQCjLvJv6yxUp3ov1SMNULPHDai0xu5kj3vE756dhfzJycv9wnRVPJAh6IcFMydRwNbmaiQZgOUm2rell8PtbAEBZ32bjPRspyl%2Fn3K8aQvsqiCtLGb6Cv5RbVxnNfOd1BsXot2jQfCIiYF7AmqOdIjHQpBwUtHl3kdXtTPGitJbQbnBanLEXt%2FD3h6%2FxU4IvvLohk0EVSFRDO2TLIMONR8lZSJ8PZ3Xcu4vw44eMKzUsccGOqUB47GnWbJzA0J4YqeKA1ZORLoQQI7pviZVA0KTdvq2OW2aG%2BGOgfD69x66QVk%2F9CxZB1TcWcDeJbDxAXe9PcQ%2FSCjoSSxIOY%2B0uCE7YwdqK%2FZJdUT%2FCTWbLVmXA9lH7kE%2FWJlH8ZmgqdLeo1poM13wz4TK6KFi0WRqhJ5BE2ptPdtpL36uwtwxjNnLCfo%2BsjxiG6GQj2RSBT5F9PV4kmYnCXYl2%2BM9&X-Amz-Signature=0cb5c7b388d5d47433ecac75c1a3e6227b4623810d2db855cc3813373090a81b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

