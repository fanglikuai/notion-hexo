---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TMR6FOCF%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T060049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHIx8VstJkzaerLDckYXD99nlumRdtFkZeQlElFMeIm5AiANJKd%2FBoAyoV3y3bE0Zn3paPXoBqEhACiEKYnmLWP36Cr%2FAwhvEAAaDDYzNzQyMzE4MzgwNSIMCq0w488CLdbQn68HKtwDSSEbbV9smNkhkDmrIMwgtudmZ3hESm9H7CIbOAQzI9NVuEqXOVBEGlnPi6MgK3JugiqGt4yrKFlP5uUn%2BITCr9lNNYKlLHURqCpHo8%2BrXU%2Bzi7FwH4qyL8%2B3A9K2ydfD3xfppdU2c7K9%2Fv45%2FlMt3bZ3M3DhLFohCRHSsm9Yvca4gn%2BQk61e2byybNADZ61AIt%2BEdY0yUsgUQUFb8XYq%2Fm%2F9GwGkOjwN4BCgRq3j6H7bd29QKrbrE9jyM%2BY9yaPyhK6qlAfvuRyStsfvdLBObvUQDYEYp1%2FS3iAQnuX4%2FZcxTje95bdCcbi%2BD0L7pdGHYsGcovWBXXRl80rkmRUhZrGS9IcRKfvEk5PzOu1ffSFUBsdN%2BKtiCD0kASfR8lCpR%2FGOqK0k4III89Sfp4csL139Fdl82O%2F7Hyh0FnfERzDw3BPCtmsgKGXr7drkucueXQgKUrYaFZrpEokrLSEPczpa4VmDQNC9pmdr%2FF2zcG67cgwzZTDa4t%2B5OKLVlM1bpPtGZj0PbpmwU8YI0tePm5AqdXpzOxKAnHwc%2FSGfIQPSvq4wywjrrMRktZ%2FrLa6Q5zCA3QDQwJG2bSKlZuI19DaDw1GKuOKubNLY7%2FwDSCPNvcO7n8FZX%2FxpPjEwqYSIxwY6pgGlplkixAVc4UUTiMXlpfusQehhWEekMOjWNzbQXuUFd0Km2aNirCTXtzWCE4zyYo4o3FHnlm4ROr1H%2BokPlwkt1%2BV2w8qJQcb8qh%2BJlQGQnpSQYjeiXlfZN6HoFPoy4ebI7%2Fyq0ssN92yr9H13irUpuUfCT%2BLNgncn%2Ba%2BSjk2U6rag5wm40t5pzLMKbiOufZQdettrmgQgsIq6WP%2FyBGefDxEE0XA4&X-Amz-Signature=9855661dd7851bffbd8d57112c5dd35087ef8e6b9f497f4ec026d8d1b7c18f9d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

