---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XXEHYUPL%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T080041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBgaCXVzLXdlc3QtMiJHMEUCIFu4V7oQE9QqVYWMGVx2H2%2FYll%2Fjt4ZUFOGAqNiL1SZJAiEA4UdSVcvQXHd%2FazVTIjAOHfrCPFg91Hn8spHUcHt1BDUqiAQI0f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCMAO9CDTuiHIDhu2CrcA0CVoPqz2KbbtRB09kpNrwxw2r0rx7TW823tocDqHiGxdcYcoPkdER9qJoF0mh4XsHuCPYzb1KJXNVv%2BlDdty57Rt06pikS8IeqvxGCLF7uKIZondcX1alGVGTfT%2B%2B4B2SGu%2BQtmmVlwGPV8MAErK1QD2439Ef5%2FpIZH2i5hdU8qK4q6nFmSrqVCY54efszGxTKAKD8IIhzobF4qkj4lhSru9IMn5TEqF6a9zev9neHt3gZBXSuEhx2CUIwxvD0duA%2Frm30m9cn0D4LREIh3zvXELbbi%2FTlJbhVsmfNosAVHcfySES3YDQ6UiRmmz9glO7RHgU1IwwNuRh%2BRnbn5ul2rDFbuNrKgt2MCJbUxoBRqqcAYnKBs1nzE35YK19ucjSeZz3q1PjH3sjHXot5tODmiadUibPpiHry1XlGKbw274uVEa4xWizMzuRTFDB5jsT3W80aDvIEU1aogsisDMP2pbv6mb8F6J5BjejGIDW90CTgKUuVc5%2BQsxK6ai9BNsVVmqU8qdW7lxv5uOwfr5VnCuRiNJSbNH%2B1KXAvBqFvsfW7U0plYJzllgLd9MGIz%2FWb87%2FsMAUaG6nZ4YFKiGsUZT8cXBDRWV%2FMK%2BBrV17G4DccBuDLd2802jkbDMKGJh8gGOqUBPjiG3KjpSa5o0vWuUbO4qPxH1197BLIsCotDz8l8O7B%2FnCwp%2BUmmnE%2BpjpT1vqnRoE0sA4WH0IDLrrHycdYZqJSzQ8Rck8qL4pzJJkDgsrRx1EMpaaFGghBClHzfpYlqxeH3gHUcIbaKLDHJ4dKuY6yaz0ra9S8eOI0o74CLYDTHGc7yCOGjAFC9B6HPN03%2FWyHlN8q3nuN7kc3JRqe0xL9d6rQX&X-Amz-Signature=014fea8bf4bd627d8c9cc512bbea832966f93550abb75d6bd7e6fee121849896&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

