---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VIOEXYXD%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T210039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECwaCXVzLXdlc3QtMiJGMEQCIGo5GcOlvcpycU0bwpmqmjFLID7GoVf4XZUVKOfKjepnAiA362QYBPZ7VDF1dmGn7O69wE4CzT%2FLTDd37XTE0%2FbgUyqIBAjF%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMGx2i3V74isyz%2FwR%2BKtwDziW9samd88eJPLlR%2FqSvmXoUXlsKhVbu6gcQvM8gURoDYxeE%2BJLWGa15qnKiOLy%2FroYKj9WZh1rH6c%2BqjqzdTZoukKmFQ0VofwOh1omWdO3xbXGIfy%2FwffXWcD8%2BYqyT8oMYfaaZSTtO%2B8p8GGPZgYyWBuhjHYCjjMFlN0qjmDtyJvXbU9ru7hn5Xc%2FJ1bITxuEXBZcQTOm48Z77RSQnZT2wTFxKCrp3r%2F9QD0RhJqIT%2Bfd5oHRmLd3wJ%2BEEf6b6yjJ2orwLjRB0j4%2FD6FXlklluLCq%2FJLeCFZf%2F7ZipqIOgtFWrkDiuoKoAfKYEu4FNkWGvz7xNDQ2qgz5ZXFTJ52a5bJxMr6%2FpHN8PglSSDSZeoxY7IVERuI74DOmBBQEKHMshk4pZEUg0cul%2BZJp4gEwszkPU6UHR4%2BXo6F5DpGYEGoXrATOhe9sTiuaa%2BOS3pfVEIfPM34tSGvYrF6FBSQt1zDd%2BIm8zw3En6Q243S22Tz%2FT6r8lHQIHtTpP8ZFESdJGjxpUrxsBjGmlgMZ54HPEOp2JGZQgaSo17rFWasrbDaO0%2BJnEbwFntKBjMiD02yHOXuWnOl%2F6K%2FMC9wzrY0bf%2BU%2FFiqovq5Z4Nu3iwuZ8iBujVW1mqUHFg5Uw3YCbxwY6pgHEOYkju01baLXMC3toEhX5VushjcSz4sccEKpJqT3qor%2FJyfe6jdvAqlRv7e6dcal1HvM3PPg9WMFQ4IWxNARj2VFggAXKpDsKxOn%2BU2%2BEsX0bv5wt%2BReZBYtfWCI2lG%2FacHaBho8iuQpxGcfAtcsjKUUfmDIDQt0nRRmSodBgUxygVPMkVMhB2InZHngD4nyAPz6oKbFt0MTJ7kKhUzDvYcAqM8ZH&X-Amz-Signature=f25ef57250d46ef563905ea297d7a12e9230d6b2b3b96653a600c8597abbc417&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

