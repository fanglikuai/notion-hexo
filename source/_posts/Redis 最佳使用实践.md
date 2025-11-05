---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VB7WXFJE%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T120039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHyVquGJuqat2HRFOlxKK6ufWq%2FjmVOcCAh7QuvsiT2%2FAiBYbkbqkgMuzqPytFLSkjzAaE3tcu5nal9GbnzbecpjoCqIBAiN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMcT4YoR%2F9FuM5iqOSKtwDAL%2Bjc%2Bn8I3JKrtlEB6JafKXImKKWt8VH2AxfjnZ2SdHEie7FEPyr74HjDhYSwDs%2FD2kPjGvYamdCcDJCnjlHIdeHj1w91cZm5mTcv1ZtiOLflSksxgpW7aBudeQ1X%2B8potMZt94o0KfDDwryWUSRvCRV1C0m43c5wPXE3BCzsRvI7up4D3kemrin6Y8qdsvZtlRRxzTAWz00UCeKwtwOFokqLaUJqyNo2KAtMG%2FLGQb%2FhxPRwC7ryLiHAsAVpiGFT6SgkO%2Fudda%2F8vpBXa5PErf0zLd4TMy%2BWV%2BJtFihy07o2qtTwOd0TXay%2FJmCdAHoKX58k32ALUickNESxVMNgfj9SFYMHE%2BOVlbPMc0gnwRSjCwH4yyz0ngHJrsw775xPhB%2BnsLB30K4ERnNszjBK%2FMC3dthsNX92LAW%2F1MHyaqMi5t%2FeZb1ffHxByyytC6h5jAOokRSV4TQgSBPvF8jPUPogbWtAgoGG24JqeLgarUZE0dEwiqwJ4Nj6svjsXERHM2%2Fl%2FqutNsBAxCy7sstYNJpEoUChDP19NspeWpn%2FzlW6Mvn6CwthLc10l5G%2BsZVW7ev8Sd5k97LhrwORNcgjK3KH43It5Wi1r1qPL8g6vp%2B8qnut9IBGCsOeX0wpe6syAY6pgEiz4dedaiKa5yE5YbuYQ3ncxMkWZIzl%2BWkGh%2BLiOA0a6HvjrJJ0oLYRjJalUYKNhFTN8lYVaPKfXOtwtU3x%2B2kjA0Pg6VgRsNkFHzH59T3xhEurdG%2B9IZfL5N0mHbeIapB89PnrWC6YrWzgjqny2u3GNGvKvKk5x1T7EEOe29nGmGe8RxgjEejJtNvYNWyDmfRuao09acbulvSbuQVjisgZmDDgub2&X-Amz-Signature=788b6a3c863f0bc35a27a4c5e898eea1b84f01c67f6b248c8f385ab1ea8de619&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

