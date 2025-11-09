---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VNXKRXJY%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T180044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJHMEUCIQDJpcMH3I9EdyX0d2E8dPmK5FCbE8ZS8CmhVO3MEYyu1QIgBFA2fLorvOVqWodaBH0CwsS4mPZCJ9%2BxkapyzCIGIjgqiAQI8v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHNxOhuOHWCtrfSlWSrcAzpYjmvRCIzeQrH7BUReOzSZnbf8gdmbYCxf1ul6wq%2BmOSY%2FN4aFeCqBQP90MBIPPt%2BZ8V5NYSMtajOxq1McO4HiNROUeECFD%2BMjygtUksjEfa1rv%2BzHfrfKsCr3f%2FO0%2FRLhtk9vt9BrlZQ1eBpW8WBZVf0gYXIG1j4axRwcdlvzEEVBL5n0V5NsmCghThCHYuFf4HyOtkezsQ3xbuzC0LoRJ8vdScJusb5zc%2BaOezV8fvdhZzsB%2FtZH9BTqK0%2B3xlFYFX45W4nsVF2rmphjJldeOwSq67jB1kj0HAdnyuly98KHbs7iDHjKj0UNWew8XkREQ156xr2ObkgQk3L0lA%2FldIaz3vWxq0JChFUM6DqWu8FlI7Ub6Vb3yZBucPLUc5ijAoRI3ogJJkGrLiCfnZ9FaEEmCEflAP3%2FYxe%2BfQAs%2BAgg%2BKYpf4bQx4l0SqjH6cI7LnD1GaiQUfdAP8YgapawNo%2BZVbx8g2RziaTFf5Tmu7XFJIPsRVIf2bcVRftup4fPfUY%2F9yhz18oxHU%2FBPCv9KTa6u1XoS%2Foy0NtUy6RcaSdZPoePKOzurspSUCumzUv9fO31zxWwM7JV%2FbJNSoOqLhgWohlDmOVCrmuE5cLD452QgOIs7IZWWVgSMM%2BAw8gGOqUBU7fdCaUgm%2Fv4s4AVLd5iMIrghHf8xVjRfWl2g5pBB3dxYR8ByAIh9OzqXbQDJcv1bwZGyHVWM%2BqnBnHai4PczZQnPezjYtivLzfZ4W7tXPn6HXUBQYx4pFh6cYZAMwkAXOGsft5ImWYKAWg8lNmViuk61%2BQoCrWSd2EcStYKv2VtwaVr9iDBdjgt5KK%2F2p6w76%2Bf3Hp0m79E%2FjtJHQlCQNxny4xP&X-Amz-Signature=48a13617e12465ee58b5bae0546549db50cb4c236aaf055c539cff6a1f8858c0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

