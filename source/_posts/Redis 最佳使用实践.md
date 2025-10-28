---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SGZYHZNA%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T200051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAwaCXVzLXdlc3QtMiJHMEUCIQCCRO%2BZ0rT46nhxDz6slY0jbnlku27tFB0DnaoFbzfWQgIgDF4OGfO75VS%2Bs5y0kEMcvtWVkbVsS4Z4hdCksayM2nEqiAQIxf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHwapwefq2kg737iYCrcA9Lx6KJRsJdelgcqNXc5pJs42uwkd%2FDtmvkSrl0CQmex%2FmtM6N3QDQvI6RxqR%2BzC6A6ooTqY506ApK7rX1JocI2RblQSia3W2bc6%2FE9x5PnWGORIHTaVOz9e1zfkPTQI3ThXzri85IdpAYLkZmH5%2BBgO6IZJSWZmiTPUyv4L4ToPseBAjbuGN1C9TlZHcziKhB5OQ1THr9m5EkERWzDKhFEmLLObU5FUTARo%2BdIJlOwede5QXCslla7cdcv3DHZ4M%2FiksN5EVxiuUTHhQaON4uQ7XFHIKoxjnUMgzfbMkM3idZowOu7F7n3My%2B7xOsrhqcog6FyLQ1F4QFGZt6sp%2Fy9HvNjOdXdXU32h6tfmFa031pMk4pkhaIIq63Jh%2FDb7tPLxonVPammlJVCY5d%2BeN%2B8nIHqk9qOK%2FnQzNfC9V6O28XBxVhYtmcKex4ZjFZHm4AlaBx4mZwm6BbmGb8%2FqS9TMNDhvRQ29rRNHZ%2BQVmY%2Bybom4IwpZOp%2FYyFKYMPOOiPv10df87HpRmJrX7LB80GfRFEqYETqQORxxRC94pkqFAwQ1gGWuaZlEuNY%2BLjRqq%2BMs4pfNKlwNwL0kn2GHsx8DrQuft6XLaTULdDotT%2Ft%2BYc3B78urDclmo%2FWqMPC5hMgGOqUBGCH7Tcq27RLL2LSUiWSCkU5Dbfxvwn3h4YRd4wJab%2BWDtOqaE%2Bi1PpWf9%2FKYNLH9RyY8cy0N3HI3erEqjARoWcml%2FZpRlnVyzjWqZtr7BYJ%2FcGqrc4nAHMLwhg1iePwPn1McUyZPSDSywIoK4zNe5DgChj8iwCuWhFUtbPzXtwIv9M25ejkojZhML6yjrJAeW8SW%2F1GVk7WeXK1miC3RlOyjxMe1&X-Amz-Signature=c1be930f9ffbdf0f697252f728d17bd04ca53da5f88982cae2ccc3f682ff55d9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

