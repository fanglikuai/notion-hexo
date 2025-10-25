---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666Q35CO2U%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T050110Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD%2FtmNh7ATe7klgqw3VBy8x6oqhmaBhuFnpu8o2cQR%2BogIgPlrNgr41CKGWXn8yKMjFL%2FgIpRH5yqhG7PPNk7N94%2BAq%2FwMIbhAAGgw2Mzc0MjMxODM4MDUiDG%2FUpyCUADmzLiT4%2BCrcAw00xaX02SiqcUtsqnfwybrC5fV8n2UvCx8lzp7hnZfzta60fcX7FAQRoEELA0OU0rsG7%2BCd0TUCsLXf9Uaso6DuQdFl3mPVKnOnjvXiCuBP5wUSM5wRCbAdH1QLF89sIEYqcCVl73pVDpPXwEOddmK1zCfJKUmSn7X9Fa4IbIqz%2BpZsD2%2B18cTcTn%2FoWtrwf0pVUrgxcLRc1bB45egoRVGsh143zg5wSqLui3VGdpqA44SqobXKTsfibU6TiAwy3q07WpjNeVi8mVBvl8wTeahWXt12DLH6gU3sCk8p3k32Nosm%2FWdZ%2F5Vx%2BJwkiCenBwLXUP1488t5SMUcS8e455jBKzdHwa1YY5ymAl9F2LyCn2iuU7BPzvSw%2FXjgbDHUdBnks874ah0pdbymbts%2FoUEkS%2BcdH5f37AfUMv2H3q46YWkiWLR7CdoOic3cXu2knRzq8WJEoE8TDuFi6Ygr5s6gV%2BbfQnNiazcCRtpH7yTeFDUbFwi1vJwQsx1XA2yY3j%2FbpuoWXhWWWpOxKsjO7Kfm8KcbwB5Pl7cW%2BxN6Lgn3ErSYCoMhyE3WKJi0XiX%2FqMiDDumTdewUyg3dcM92S3ms9UFmXHuKM12VBIiv6DGgUoNkka5jH2RKRvc1MLCs8ccGOqUB8g5ceQ0RdRmvynwddbhUWBabx%2B5N%2Bfb0xjUDIFFsH1bRJl4vfQpWhwtJTIs01rmQ9PcglAbvT6OqALahZHOv7ZrTuDvdPiPRaw%2FPlhwu6s13SS6aMNk%2BU9yFb8YR3RgpxnylvGwZW3lJHMgcEZo9Z93iDMjZEVuzbzjxcule5WEyC02iaj6X%2BL%2FSRQEQF%2FUVEupvVud3O7KmvmqcbCSsHVrSZK2E&X-Amz-Signature=a12d88f864e27c43fe240b42858ddc38ddbc334988b744e6ea81dc0878c07f4c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

