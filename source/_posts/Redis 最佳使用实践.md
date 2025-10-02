---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VN4VL35N%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T050038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDQiGw%2ByZI8RIfl%2BC%2FLwA7K%2F2mnmC0tPkhFhTKaMcPzIAiAuZKv0jVyhY6LiDcCv3XFyTToy%2B47K2%2FOWJHASWSlDjyr%2FAwglEAAaDDYzNzQyMzE4MzgwNSIME%2Fn%2Bpiz8OYhOmtRYKtwDj9RF%2FRwjw%2FY8eqfFRJPB%2BGgJ25vQqEXD2dTMRQ1oIJEYemntDdtL6j8QM3ejQvz7SjKb1NtQRpJ7gduz4BopeRNj0HHtuYxBUkdiD%2BbQKpLZ9YStYldjXet3sICkW37fQfpIwaq58erSjfSIsMtQwct4EYHmLKE8oyjU6wOSLr%2BQBZ%2BRR5dLJwJa4Hcm0r5H5YsIfGtMdY%2FCBv7gEiMn59UC4CPfo6SxufufqW%2BRwXGeoucoqmoaq1Uqjq8nUrmfiyno7fxdxuqTOeQIuBgRmJG2S51dbvVOYUm03AZlQVlWxQ1VF4wG55oRpOd3xZtvzuxK%2Fiqs24C7w7E4J5Mmn7SYa8cKnT5JQAD9c%2Bv1HnG2%2BTIgW%2BCUkE1KMDUR7iGmc%2FsarVwPxAumfJewd%2Ff3VDNuRvYjBeGM0X%2BasRx1P81FDaTexuszaSKic2yOBz5CZGyVxXpD7B8av4UiuKq%2F3gCO9BbqIy0i%2BCC0%2B7QIkczi2vU9%2B2se1Sccd4FMo9fI8w%2F%2Ftuy3vJMteW2rdWOD2vI6yk8pknv3FiGUQg88a7Om5YbUeLJAU8omj9Ab%2BMs9Wbwt%2FJC%2Fg%2BDQV57V%2FbgGntM0XgBWq7g6dsVsJ3avn%2F1a8je9AtiixmzcUrIwtvz3xgY6pgFYjDwJnElGtf0Dy0ix9s96Kwapta1BprPJXLypZR%2BqjD3G%2Fob05dPF7F9i27F8TEeZQcVmUzXUONYhWVDXTXmJShmQH7i2m8SPTv9ddyx9SSfof6TFpvrv9%2BMBfrrLheGud4JGcrvA6xmGmha6q%2F4UC3sRLyQXpoiTIGheh5okuDbSDTp8DkjI2xeYWZXX%2FqX0f41vB%2Blh%2Fsn831nOf1%2B%2BnWw05vgz&X-Amz-Signature=250b2036a32b4f8da26acffd600dfea1bba6bce9c4943c4c5b1e7b6a8c3562e5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

