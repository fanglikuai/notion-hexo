---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SCH3E7JM%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T120043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICM7lQX%2BMFSmCcH3rG9kZZnpc3dd8jKD3U9ZVBBpJv%2B%2BAiEA0Sh9jOo0fjGrPW%2FIkF3mFZFwFvCAeUFLrbR779dWIpsqiAQIhf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEKWCv%2F1GG0OTszzYircA%2Byy1rBjqlH6meOkv6HOECpGbVRygBUWU%2BUpIfVn9WjQioMVRo2r0YabYfmO7ByUgKKEj7nH5q01u8bAIZzbu5wG1hJCzVGBzPPQI6bvnv4gfRAp2RjlQWZ55t%2FkrDsLB4Fw%2Bp5CMrUx98zOZ5MvYcnvpxCCT9k%2BUvisTVsH%2F5k0o7PNe0JicRRG%2FKOVWV%2F%2FRAAY4ldy6m88OqPN2HI85GtBBpziM1ahdYa5cJlpfC4om2ZmHNwHkWYFupk0W3ziQRye16GhAOkGUP43xuiUWuctyJt4phckYNFa3zCo7oFDt2JMQ88cK%2Bh1GJcO62zEgDYigDwOhicGxDDmcbdPSV7DB3YJ0K%2FmRNXWYbJ1lQ1qqTr4UX%2BdsJ%2F8mYgMKx5gjq25u4ABJZYij74RfMYy14a1q5rbSvHjKlcWEaUvRd5cJRL%2F2iwLEbayG2ZxMx5%2Fi1BeLyMpG8wminlHvblSLt19WV%2FDqTNBAr%2Bthr%2FN4i94rqpToL3hbaKi3EP1lYP0gx9O2g5n%2Bil65mIEAK0HCx9GxBKNQ1zNHy%2F0uoSJHsPNsO7oty4m2WyHUIKtGF%2ByJuol7BethW9ZoRai5oOzYSDihpxPnuc7WjWXF5X2NWp9vf58%2B5cKIwbxtuo8MJXKm8kGOqUBQP82l4M8URZ83ze3Hk5%2BBUDRINGv1muymzq6tDZ%2F6fCehtdWWxscAwuppTyGUBU4WWjdiz7mOrIkElYXeBqQmHKjNTc2fCW7nCkUeNZEUxLhB3c324EfpVwXZYiV%2Bm8IO6JLBycJUl%2Bu8rFtUV0J4aBTwMTS4yBnDsUvOIL0ZIGIgl4UFA3YNqujKm%2BjDcbAAO5ANppR3nwmFZFKYR3DAjcPYI81&X-Amz-Signature=71dfb230521e1b041792663d27428733bf84a6c489f2e3707fc7b04a10e6bc87&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

