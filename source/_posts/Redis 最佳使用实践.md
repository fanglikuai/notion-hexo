---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SG235KQR%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T150047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDL%2Ff0guQ40bP0GJ4TFA56TivmOCUNbARS74rFhlEDqIwIhALSXZND6Pi%2BW1WtIvJyH1YIgvyJ%2BLtbSmAKNLffLj899Kv8DCFsQABoMNjM3NDIzMTgzODA1IgybZM9iQJqc4%2F4Ujygq3AN12uneaze98miNpoNqFsRt3UFjeRY6htyGzTyeMEmqUnHNRGcf9f1LmzwiwfrtEqKM9Arp9MDifIDkKBrkYJAd%2F4DKRkXO8LXuOZbUNTm5sNHHVLpjtDbBNpty2dyNHO4zYSvpB2UUV9yLzGZDtZQviQRpB83eGqXj0Q9i747oCIZJQhCjeBW49EaM3b9yBpbb%2Fj2WQwNnPMPE5e%2FF7R78ZycL3v8x1lsCIabNf9Y4QWw9Nclj7yftMGEFnPTVSincidtbzPUxs0z0tboGEkFt9IFD6GYwBfR09oT74vlsODI2Ua%2F5M9Omz4IdEn16JGRh7F%2F5RoNC0%2BW555nOPrbrK2%2FsjV8ur9ZJOMUwPLODkZjD3imUGTEX8LbZrBvDuEZORRKjNwQiVsFUtV1S3OZdt7Z793B8%2FDivok9T47uqTBLe0ng%2B1aNUvZGtf%2FUBGpS%2F41i9dlcfOizh8gA2hlyOskVWJHcnvbOQLrSUufLdi0CC6XCd4JPEnkJ1JiEv3%2BL%2F%2BXzMpwRc2JOxEx243j3%2FvlmGB1UjDv5LNlYUNdF4QpiNbq5opWpCaP%2BqArIIzL4hvWohQVQaoCxmt3XLyvu88q3rFygdZDkOAo9QKzjRyzVpLktJowYu8Jd%2FSjC%2B4IPHBjqkAZ87jFImGZK63PvP2RGP8M73KsU52EFUsoNQFeaLtzaZoxx4L9%2Fsc1%2Bn1je94LH89xpvWSarTEw%2Fff9J06LOImEUskb8WkJGLs0hx8hKUr733%2FIpKKHOIBYNRiG%2FDxsK119qtFO0Xom7LLW4YLS%2BpR6%2Fu9ackkiF2xJeFIbS967KnNJEIyCqH2fuFH4YDjQLzLpLJSue6qg4KJ3GFjI0dUI9nhf%2F&X-Amz-Signature=bf5fa55b5ed10891a59bf42329ff43e834e377b1deecdf0bfc8de0097d9e6388&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

