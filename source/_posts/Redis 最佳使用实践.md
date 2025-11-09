---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XNHK4AU6%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T110039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECAaCXVzLXdlc3QtMiJGMEQCIE5gHKmLl9j2bdIcfFSiRfukUqkpK7wo8jotqO9SyvjUAiBKzNs%2FJxOlfVbyEDvJ7CX%2FKger3a60mlvf%2B4130qPPpSqIBAjp%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMrkJOu%2B%2FQWkIL10eOKtwDzdP9v092ErMDXPNTPlj3qXlThiooP6vN3811MLREbBPWD9Fz357Y4%2FjDX4h%2Bde2DIFNpqcqkaiVLHdXbD4Fu0X%2BnZ3NtOsFrFfOoU6sBd3r5%2FxZgBeatdPjufUiKCKYJCTgi69SK2NUrRcKopzPnyhNB8bCiLhqkGqHxDgHKpEnEb1A7topS0FeaNQ%2Filk9hSBjunR2I4wBvtqFifGlpMmEiz12EFxu5U99JqqddRU7niiFw08C0OdBF3SL3LeutWxbxmb653bfpeWTnurV1l70s50r%2BENB8Rj4zFu8flOIa0UobN2dRli3Eb%2Ftky7f0q19Sx6P39FAuza580WZqGwl%2FKcS%2F%2BxQrg%2BTkpjzgkpjnaGuQpO%2Bha6gQIXeL2XLjl26GWosU7HFny5Z8d4B1Hh0grYkvi22bLde4Uauc9OYcuBV883Qi1mdBlPxdwg6KE085mIUIwImychxStOcq8lHSc4GceNWYfK9UiDXRrxfShC%2BgGwu%2FjNGzaLkVn1PF8LSyal4ub52U6PsLnKL964uNpL4raUWhQnFNhSJOFutmlN2tWrHJ6rqu6Vpwdlil78Ipw31w9qWinx5fzKCLgpw6%2F7NMF4apQMLFowqUajSGP7bwm7O4k0DTWjMwmozByAY6pgHgbkeutQmsR%2BmUEIZfS%2FkB6rFWEnH37Bhdj8J%2BL8TWHfo8KozfacnzuHcuCw2zFOtRZlx4N466dxIQO2IT9DXFO6Aw4rZmo%2Fircw1ivYiOVD7pU9FvKMxNJxBclBrr%2BwJ%2F1Av7NdWO6WGmYcrO1yLmFLK5SckoyXTBg9V9Qu33dNfzonDmYngBj6MJN27SUeZfREiMl1eXBSDvTxrOXBWZNGCC1i7%2F&X-Amz-Signature=d368d19ae75cbb7af2198818f4c40e942246524a1c8a235c704496a0698c976f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

