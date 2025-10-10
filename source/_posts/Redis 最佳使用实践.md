---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QSSYRMB3%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T000046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEgaCXVzLXdlc3QtMiJIMEYCIQCKoBQ1KPSsj%2BXQEb8VWOGRnnubsfi3Mw7YG7efj7I%2BigIhAKj2vy6nvfM0rminywuXsVEKkn4z26jddMt3ERj9XX95KogECOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwfcmV3PNOOKTqoUp0q3ANRD3wm0bsfnf8yenmNVIQbBM7RlS09kxyLIUvnvYmbhXhAD9NyE46dbn5FlY%2BvlT8Tgs%2Fgcg57Z7FxIBQhdWqB28qQOEZ9nY78DB%2BBgl7x61BidKJx3c6%2Bv5gvD0K9ImxQuh2%2BLJI6qWe1d1QMu2UJXXlswK7ilT%2FGda3w7Uj%2FvA4V5HZZYc94spLS87VZHqvdFNBZxOCRLd0nl07P4sjFMBSltrSPKLiSCYTPA0%2F48%2FuZSdA%2BoxRBCURbb5a7TfC%2FcSlLNMsnC0n0ov%2F3UWvTBYP1zOpualuay8dsQAdOd%2Fv2kTUTVADF%2B4VBFffa59ymRrglwNp9xlwQXW0LhSmr7BYncq%2FdbXsm%2B930U7aKzO2hDBaILN3dRQwaoo0hwf0N9rALCn2mm5EpY2lILE5%2FplTqbsES%2FlTfHegJPa6sARTI25%2F%2BzZQax4TJecc4btqL2MsbG%2BQu5Z9OUQMNN%2Fl0O6HUgxWMEPoRPi5UeQ6GtktZs0VGPTT3Wbavny0A2cRXqaS%2FUFIWobFDDGwmRiz72ZYwkwNre%2B0g2fsVA08n47MvHz7tj4enmyLWGS1Ra5FkHMI3J4BO2NLB5QUjpEm2hG2BGJYcIQgEPLaG0GsZCADbs0I3lBskoUv4tTC%2BkqHHBjqkAQfPIl0vb%2FDpm8CEDJ5%2BVrZv2Ykr0cNjU%2Fy98y8c4Dk%2BvIvZTwknqb6oMKP8KOWwIypYr2xaRBc%2BA2FRtIMMGvh5hwG1JLrOIZHRuCtQzPx%2FByhvTP6ZgNDwxSVtuB2n9yz63xUPz43xzmasZYCrf8PT82ZbWJbhdW89H%2BHH9psgB0uYGJ6crkGWEL6EEeBq9U9%2BWHNdgGK%2Fsqk%2BxRqP3tuNfWgr&X-Amz-Signature=eb56012946e52f0c57caf5930ac8c7cd0d24373c3d612f21d06ae62cd00f84f0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

