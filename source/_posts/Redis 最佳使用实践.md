---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W3RNPSHC%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T220042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFYaCXVzLXdlc3QtMiJHMEUCIQDPG%2F5Sna1A1%2BVte2LhNBVrlRWX4Olx9TqPVs5E%2FMP0IwIgSVURIy0FOBSjfDnPuLOE7QcykHkbFWI6kB7nA7U9k6sq%2FwMIHxAAGgw2Mzc0MjMxODM4MDUiDI2xrquAzNcMoUrw3ircAxOx09oqUGU5GnuKoc%2BmMkUnZ0J%2FZTsJXujL2uZr3tlhQzMRsGXiFsfX2xXPDwrveC2uPHwaM6qRo4zhBj7H4%2BjGiyDdtxHoMFXAYcorjjt%2FIeVtbHP8XgMF0fpZ%2BxuMN%2B2cVAffnOD7QIcpdK1yGr1PljU8F5J0K2QSdlO73U26FhYnwwImlJx4FC9jgOs7FNz6yKL8HCtekJ1WhEzCuEykoLVONYmfz4%2F%2F62a5GyIVXFzsBweT7NGP%2BMTAeqPBT64CY0N2ily4blHRgfSA6z0zJcUbdOrMiZ8EAgMkZ8YbB6JDvxU1EKDvvPxCiIbjP9D9C4d2m2qThsU4kz3zeyXhexoutUWU5ecB3RWOmZd3PsdL%2FjrUw1HK9YcuOPoMCcl0l9dqGXriYLVibwqI%2FYcu0I2z3OYX9%2BLj2J3Pva2kveY6yAb4RHBo%2BnaVjCVtlQKFRoNfXF3WWcW5LRQd%2Bcqpm%2FiODH3Ff%2F7TeIRahQHw0f7ZfKkJqY6XCnNekFCMsY5hZTbRh%2BwC4KduBX537S6pUeAqiBj0Yn%2B5rrbbEQFyL6U5TDYxyOgrwI51ovdNIYF%2B0Hzavssm10Wb%2BQMI24obOnRZZWGHOBXnC5mxz%2BqUBXmB%2BDUJI3PID22ZMIDhlMgGOqUBmiTVuVApx3vY%2BQifWuZy3yVDoIaOeWHYAcoUo8rsTVSJS2qhRibbH0pSo687jG%2Bo8KSuQgRwzjflLpz7aA8vP26haeHCzcuOIgzlY0%2FUUK%2FkGdNGINRAJ%2FA4LHzuRNlCzrfVj5D8KQ4MJb0Qw0oPGAUv2c8M4IXmhBdwkfzpxoOpXdPe7nDmlcRkY2rpHrgwMSqr07yXmCuHFVpAcJ4ieu%2B84ZrP&X-Amz-Signature=a0d4c41f910755257a47e66e59550a97f6c4a1bbf63c2829bf1cb38e05f43a61&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

