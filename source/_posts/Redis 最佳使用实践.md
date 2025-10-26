---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WNBQD2C2%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T010053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGU8QH3v5BaJ%2F254Yzaq0IPpT6zq3vUjW7kNROTMQXWuAiEAn1TYROhDkTxbS8xChfmQKH5Qa9ji7BUe9D24L%2BLOdOUq%2FwMIfxAAGgw2Mzc0MjMxODM4MDUiDDv%2FAJP7mOtZ5wm4ECrcA3Mzzk0wRP8j5A02GMI%2F6I0yzYwqHXmYnqS5yB1ajNyVTigPeOzTYs3GZB9eZyYeAiS3yow8dokPDXoNX%2FruVHNLskZ7SV5g0MxTPQ4umrnXUuot3IO9ZNqf9Zw8Uj3nUkTpFFjA3HCWdKCJSqVcvO07Eg1v3IiEaxQVR03kTukYO%2BWI5blAC9mglSZEF8hfDWk%2BK%2FjRCzyVaDAb5Kaxz3LbSSzLEACcU9wzK8fRVCUy66HuYOWwtbR9gQkz80fVALWdYbC3Pa%2BaMuDyL07mYcw39%2FZ3vJkuYNTerpM%2B3LzlTxBcJQ7a2BDOK7n6WMFhpFxVE4fKAtLsZ25WAkGe%2BKN2PA8%2FYfk3FkbPKPkIYemg%2Ft28Vwirk%2FJnBfNnuo%2B2FB3mrbTnJZJ6%2FQ9ETHi1wZXgf4WXkbafXNsPe8Nbyw342Km%2BHeQLFhqEbgz%2B0jRm0Yt0Qsqox5enVllMuZ5G3ZA%2FqVA2QJJXpUlyaRiFsntK2k%2BQT3Nrsu%2FPk2DSKBMrE9QyEjos4MQetbAaV8%2FWyKCEmb4IeKXiLK1VaNjYRNZiRv3yKbQq6DJDgVoh7YUwLz6I9OYeGCumVCQxX9fPyyl8GwLCynZtL7d28vk2JIlnOzDiEdBabMGia1lzMP2Q9ccGOqUBUEngZPa1bwoSCJKVbykWV0bKRIXdswYLhBwD0bNYXchObuhdwczWSaXuLg4pOcWaY%2Fnm68evZ%2BTIlEmlTGnKksX6jtFhAUuIiP%2BajqiKImd1I58zgD2NSLqq7xiIlrXUFC3tMI21pI3ux7ANePtw0ayJFzg8gBdCmYjDBa3G0TWIWUZyqtVjX%2FusMoyHUTpjVBMcowXhIvSyt290KNczqYZrWn9Y&X-Amz-Signature=84888aed863c555e8dc6b6f29e72ba702a44a68ef25667e1b4a24d680ae9d392&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

