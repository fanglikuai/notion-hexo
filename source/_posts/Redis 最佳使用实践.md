---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UTUBM77G%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T160044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBAaCXVzLXdlc3QtMiJIMEYCIQCM%2FI9ARE%2FZJ5GH5IeNcSJpj1OD1ILnmfnc%2FOgVyhS1EwIhALqgP%2BOPlys1%2Fa9GIG01CiWFUMI%2Feh1iBHGCUsgSn1C6KogECKj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igyh4byXz1JT%2FsErF4Yq3AMbMucf7DsR%2BmzFJ%2BPI%2FymjGH%2FpXsadKuXBEiR7N4%2BgB1HkRfg4lhlC5VuYH9D4AmTh3XWLX%2F3JPj9N7erRHKh5IqDkCvVJt80wM0jjwJmIDlh0cRnZLg4hIZJs%2BAysT81j%2FAm6pxYCgauOUe3%2BKtlQlGzw2oh3CLm4o40bF7ZxzSjN1ZEwHMkEkDT%2B0bvvO%2FbLaUJXyBeLOqB%2Bga6UoSl6EucrXxHEG87TG2x%2BpDYtkyukeMF35rgdxJHCdNBT6lZvbk0Vh6%2B4uBiHqw94bu7pMW34DtreeR7bLSwU51bcitq2R0HI%2FsxLF7q7SiGQDQrKvJj%2FkaH3Z0OVWIASltIT0gP2z9ec2%2FpXikGGT8CKZoA3%2B5j%2BpWaRt6EWQWk%2F0gtAK8VF5JQgResfcoe8gxLgt2p6W1Qik5yxrxa3cDqLWib6G6OY5uRxOHUGTc7G3r6rU%2BY5%2FMSXaALI1ZZcVTu2SaQyLfCwVfJ4bMY599jTohp0d7O5VuGZZ7N9Nfj3vS0PYLzb2aCctA64XVYLXrKpyBOiD59BaSG1ZgYxpqjNjG8EuuRWuyofLeDSaVTuTmBzAQ5m92IdnCywJmlGl0kjTi3JbzRz27tGJ1lFVmWXPZTaF0VEIb%2FKobD7mzDC4ZTHBjqkAYLhxssHO0SAHzFkUwu3DaohLR0A%2BDyTBmTXEnGtLBsIDjnMb47Weg998CDXurYeTRJe3JalazTltCZxGF22xpqBqdfoIcOskNpOPBA9CEE6LGowGShZu2oiGBGSPE9U8dBRi2r4PPb21u3YAEvYjbgSEMaNvRfob02JFdMqI7hbpfAeLTLhKm0URm3LRU2HJ7fJkyJxeSqjCwYe8ZcJb%2FORSZt0&X-Amz-Signature=b80dd2d6fd779a3c600e91d347db45dd94b5c9be6aece2357eef364cee755096&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

