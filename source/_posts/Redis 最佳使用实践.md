---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UIV2EQK7%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFZ2vLUgFCDpncRsV13SuVnl1FkyEg4vFXYJ2NDBLWHRAiEAoAh9xKcpiMR9EzZdFhWxV20YYBQ0IzgbXSSmKx1rqOgqiAQIl%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDL%2BlmowX2Z7kSjMaTCrcA5xS3KhzX%2FT1mzMZ4OtjPQwrJg2w2V7HM4%2FegGZ4b3AysC2kiXvel7T2etgSdvBnRIZrKJpc%2Bdky%2B%2FersRtN7r6gYJDjmQ00MnPePDB6%2BYvRqoiKRHpuYdTx67HjCy8JLrDNHX%2B7MADF79yfkSTCKDC1AtFgofUrZEMH6Q%2BxavBIym0CAwMqIIIL26FZuWbROnrwTc%2BmFUzfXga4TaCxYAIrNismZwPY1t3vDE7H5dBM7PeZtVMEq0WHPv7dhKN85vUGSc1Z%2B7e%2BIJ1we%2BK0dIbMd4ICcsBsY2sdIgoUL%2BkcjhJc7Wl%2FZ9TWjrwvipaxZFP%2F3N3F%2Bof8tmG7AdMqYUxmVWfvFdAerdC%2BpzXjnlnVR85QFVLccrRkB%2B08H6ACecddZ%2F%2B1u6peEgZBgEt5fPNi9Uul6viKrMlFXWdd66tCJ8PDysskrRjLdsTgwjt4%2F5OHMVB4CkP%2FOqyod1oMEZbH1%2BsYJXFeHbMSa%2By1zlxlrEEHL%2FGIaAmpGodyuZP01HjM%2F%2FELBsqbd%2BQK4wiRH3doH1hrUL8wjs655Zc7WZBeKz79viZsYIXBBfufdP7UKyIaUQC29kmUBkfASmDTt22xlgDp%2FCastZrvTZ8EqVONROlU0HC50Vk4rXPrMPb0kMcGOqUBP2tc1QtTTrYGUSgNdlZ2guif1JxreTBlAgLCDqjnOzAOkBlVeyqVC5e8a%2B9AcOWxlJ4%2BykyXNLlLcvF63ZA2soNV16QNO2z%2BpfIZ6ouZnDSlLSn9Dtiks4kTEScXB70r2DTN7eq346vxw%2BhtTrCdp4KtgrvfUmfuQ4E1NWHlVG2nO06tEPpGsuMrGzy90bpfwkn5dglfGPyVilH57aNSXYbWlreQ&X-Amz-Signature=d2b36fbe3b67a2d608d1a61f9cf297e29dbf9dadfc8f84c23ee5870357c5fda8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

