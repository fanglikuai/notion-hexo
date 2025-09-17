---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SH7BQ564%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T070049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECYaCXVzLXdlc3QtMiJHMEUCIFbk2J5%2B0fvu5VaxgMZW1giGPNARWdKvLUT4AxYJx4qOAiEAn9I5oCPWTcbmXZNuuQgrlUmb8gwHQ4%2B2kxef6cgE9rkqiAQIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOuF%2Bj1EvTA%2BcLcYKSrcA31jr9pS5uDqlnwN0uX2ciocbbjB3sDNRTydw0LYCDMGXy88tn7P8rksekFgGwH3CWH2TktFzFubdQOGnQeVhPL5LFq%2FUbeqXhRCgtrtSVX43Hvd8VAF%2BwAOWXw2QHZXJ9PaKxCcjI8F9HGk28RlBhbV7Pf78s61QENzYslMIBBtSYq0RCpiXPRlTadDInWpcKklZKwxOs74AnDry1aWrL0msMvhT44mh98JBSwef%2FmGQXltAOpsNQTiQ6pD8laZKoUxaPHgEeB01NXxzWLHs0sVZM5ljOsriV1BZD3whWB7hNm1rLHn5f8PmAIEGbbl6YkdYCH9cjJp34zWPwZZX8nDVpeEJ4vc%2FoAod3iSyRFI1asmDQeTIeHLqSAcR2lAeSK1OOEWP7Bk2%2B2dS0eSlK1FSGL7%2FXKGZ9ZCFoBm9BB79WVss5uiMyqage2ECPXZX7Bfv5BQAn4Sn27Px0K9LE1t23i4wA7DSTPdfORjHgFEWl0%2FnAQNmnKd9BRl5a8rp8yqiFV3JFRxjZyLuJxpnRI5nQQG8UrDaFzeObFeBAya3fiy3b5DXsCkogMv%2BVCm1i8Hii0iKLTW5G8bSxX1y7Me9zytHz86G%2Bb2y0cPuv4ucgj%2BlCHCPbw8V35QMLikqcYGOqUB7Tc9yp4LfCR%2FOHxM4o2i5cQ5AdJ6QEOEHe86ylCHD9dGjpB0hCoihSPcZe55F0OptK5m1CzeodHSoq%2FBdUHSp%2F7JBpt9GfuhvTHNRLtigY0WM5zdV9HSsN4OC%2FxtZg%2Fya%2BBVlUr1LMcqvgZBGmS3sZz1nJOyjXEp4lYbHnpVumOFX8fSLNouAppIMKzxkoI%2Bwg%2FxFjzHHa408F8RgjJNqvq%2FKt7u&X-Amz-Signature=936e69d26298b610be7d61e0fad268f203b76332afe252387d4b765cfd8de135&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

