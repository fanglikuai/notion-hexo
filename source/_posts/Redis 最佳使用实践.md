---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46627SMQV3W%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T170041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFurg6QodyscnITOOGB32BHcEd%2FucmiA2V3cG9BeVZ1nAiEAnEYbrcIPJ0fSdoV6nnWc9Ji8AL07w4%2FtObafCe2oanIqiAQIif%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDG%2BfEMvC39vxZcybcSrcA5Rr8%2BqoTIdWkqFz%2F%2F4FfpB06N9QVXOj5dGEFFzTdY2vDz%2Bo4UvRxxy8UANc1lpiNh2Y1veB%2FHdIHuqOvXQLX0EeCOmASNQYA3o6fGK6JuYF5LEhyP0GGtFlKTp90mX7KspnjPr4XeS7ciWZ8SaY%2Fa%2BAf82KzgkbZJJUpOZIpEFu8auk7mav7XRJh%2B1WeryV9qtt%2FGs%2F4zrj%2ButeMNHPk6SdBK6dyaBmQicu9r9f1BqcX%2FrPKoi8Ck%2FLZ%2FkdMoN0WfUdd3%2BdrSonLRFX1Fr%2Fegdp2Ga4xQ%2FZ%2BXC3keGP%2BPV5Jb0Jbkh7yaChDzOUaYWMKmQ%2Fwu5coDtFqogzrf6SPK3%2FGJWuU94dGa03AmtvecZClmXABmsKBpmzsCwyZGAGLw6914kcfk4sIht%2BNesIj2Yzf4yesFZ6Tfzv4s2sYjzO3%2FUXah1COye21jYYg5KWKkJs6qKgviasfaPGyvfZ8iO0AdOBv%2FEjSQq1qDfdUfyZXwI7aLTCZVC9CRz9QB25Sik8A3S0sNR2UbZustrrpGFLmbJi%2BpK2iXsR%2BWzRiDFsagOy9Nu1S1JETtsEJcF8YB9Qwn8com2fk3lN1FuXwLi2BJOeSClcH1KnAEhzpnoV4b1l%2BPAwA5ziSvx3MMPGnMkGOqUBa4zUeV55%2FEWRKrEBGPNWzaIQ26bZmiDTwBXAl5eFKfVKId3O5JjDJLnzAugSkGXpzhD%2FN1mb1K58vE9Qu%2Fu7u4657pnVipgG48IbNyn1DWb42Z0WC6zhVB9Kk7O0iH1R5BD8Ip3AHWoXDv3JC3LIjQxEzv1TwX7Se3GOibZp3Kx0Sr4KZk9I3bEpNxcmyxCWQfyBtnqkuJwiaEP80P7zn8SjFXK2&X-Amz-Signature=934fa33deb6f9b9bb469237a487ad364e043fd29c7590acf7ecf56330239a96b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

