---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QQJWRFKS%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T010041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDysqupAJB35FtAxrn8ZgDAC6ej8wjvNGGPNl4HlINgmgIgafv6%2FzTC07Zm2fPCyKwxsI1N7XnOd0udMK%2Bbeap%2BNBgq%2FwMIURAAGgw2Mzc0MjMxODM4MDUiDCCuBTFxbFysILPS4yrcA1uliV2mxjkrbdfI%2BzQ325sSCEMUeR%2BsW%2FrDt1PZVuEdSE4jUquqBB6hqbUHwJThgPwgUQIOSFaWtjugA8Lx6vc9cZC%2Bi4Ixj24svOMIKjBIMzjpiyIYkHPfZMtPfeQRqq1u4KKIW3p%2FdY1qRNy%2BjriHAWcTWnvZQ4GG%2FyOvoisiE8xxiYRyR6dyzwazOFKtYXKjDWnWbF4PybuQScUwEzgGLU3j0xt5neruJEyFUH3A0P0XRjy0%2B7Pd08dK%2FG3tYXMkEZG2dbKXPNKDUmqo40Ki9hg6nXVPYJEg%2FkgrQHe%2FT0fszAjRA37p%2BZYt0gubIlNB1j9adm6ivWhpGefbkzheWZkN3hpAmasOX8VlN6lgnxdBjHdvP4siylEx9n%2Fm6n3Tpz8sJrUGy8kA1Pl6gIcgB9OyACMWyag9QdwE%2Bsrnqy4ChrnK1kb1brlZyibbuhS3Hc6sYaJi5Mez37t%2BmLHtGxsQyk2x6dKsLa5YSeMuRYdOWLcgzW1M1aLH9Ov22%2BVX1u%2FlOcfm9O85MoAMAdrsjMq%2B7zIBSKF0NDw5E%2F3GvVvwTRFEmBHTHFthMkO%2BSUsx%2FZSOUfiU%2B7pCdoqRAVDOVBawHzbzUw%2B%2FXYHrkHJ6YXF%2F%2B%2FSwXzLpM0pSMMbnzMYGOqUBt4L8oyNfSMkNqCUG55kcYQTkQP2QuADHfEQn%2FFiFTxLMQ6%2FnY%2FgYM1cUT0W%2FNL4e8FPZnCqG27FjrejiGsSdTuLruMtpCqW83Pu8AWK7uB7jcWT9Lj%2BYlv9We%2FHJWtrs6g%2FX9fjsM7Cp6ZSC4lFtIeeLjcgfjHDc%2FwLAsr4W%2FzuB8Bh%2BONIm1SNLbXFDgIyUEe8AOQkmkF%2Fc1XchzyTce8qDZJBP&X-Amz-Signature=5feb3826b16f43b9aa506e27909a00ef7bbc4c8bb14e0de4f5294fb99ccbad20&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

