---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664EZRVJ6P%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T040047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBQaCXVzLXdlc3QtMiJHMEUCIFpFOkSjjBx8D1Y89ubEMHrDnRzsqHbUnExq15IvRoEEAiEAyb37Nk8UtgIRoo7cSn5LxZSTGUtL3Zq16EbiPmLuKKQqiAQInf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAshf00XvrXAAIstqCrcA7Rfn3ctEZCPDf0InczUr2nE9TpkFy%2BaI5kQITKtX9E5GGwIVaw%2F7Nu048c7SETowgcKdbrtOCcAfBptetwpExJd7s9dortCWQ07jqd%2FdCH4iMJMC4810V3LDAMOoEbDPlWR9PAu7Lh%2FfFomhFBp%2B855DNTY09CXaV2DPk8ovbUr3r1e27oiLRtO3PamYPWy%2BkKrsUNw5PZXlAnN4hTBQQI7kBSPP1pxIxW4SnJby0IqY9zQaEiZhIkGvf%2Bb2%2BZKNulSPViGtUV%2Ft4Rzfrg3Blw47cIb9ifmsuM87RtvzgaLc0sOgMSygExn0lMQYSXnOo7gR3BEdfGWe51T2wdHvFsandTd%2FgNeaY799BB3OeCBp%2Bh2yR4h7kqmiWgImgV3G5zJB0geoxus61N%2FFQFGqxJ%2BRuy96IezPMlSeEBToXdnLfyL9tyXjZqvoeGawdMDwvh6x28Jd%2F7ioGvf%2FCXWpOqId7P2UOUDPAyNCXkC9ushlB2qETWY%2BRr5C1MQ2OxITnEPjQYJdh2WsmKL32VKY6erWCWJiY5BrFX6mk7RuSDi8dZ9JYjaQJ%2B8238d2WvPHBIgN0gA5FVPy2i2stB%2F9oDnQNp%2Bw84uscYYVBgouHZy7OG%2FQs7QgZR%2B6aMIMNG83cYGOqUBWPFK2H1zTtIosTLgGBvjr%2B3c27HKQ2%2F0av8eQB6mYQx%2FakLRf2bcMx%2F887BAqxeqVWSBjgym9OeWIjg8rKN3zaeqMmxapaE1YLTqbQU16I9alq6LIQxlHh7rMRGEVAipxYbhnNlcXhu1u%2Bey2e1k9hpAQhiPiIrqK9Sh41xqgZRvfSZRhHc%2BuWgFLd0uiK%2BwiU4IgxTYtq1heTy8LO%2FV7s9T96Qb&X-Amz-Signature=5ba4c11558042a2d8ee538cf396d41a0f46eaec70e65a40566df58b8ebeb6eb9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

