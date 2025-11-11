---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YMHWRFFD%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF0aCXVzLXdlc3QtMiJHMEUCICJe1lfU%2F2MCYSLLgF0mIwN%2Fn%2FaPKG0EYZwdDqPFTDaJAiEA5Pi4Sg37uCB1e1lRGnwWdtrboafq7pJcTwHANgwb4Zsq%2FwMIJhAAGgw2Mzc0MjMxODM4MDUiDI16lTsP25%2F5ukOe%2ByrcA2gOY2lJLqNQcnD7L2D%2FXMon522ZuXRZ03gOW9asWMOJlqLTTsex9XtU0uzdeZiiTqj9y2Nkltnvht14q2wPe7UYLzqFMqK5DSCHJwo%2FASZw%2Fp%2B4gVlAQ09bOkbmPM%2F7EQnO40Xkw8%2FmqCez6r4izY%2B7NAr%2FkcX3Y2o1w%2FdiVSll7TvX3Fk8nyGy3M4mYQ29wY3N1GhIQIfKWrw%2BRSPOVGdFbnlX2u6kXHV9BeNLVOJn0FHpls8ZpzI1zDTclbSAwiFICsvo%2FCX0G0SCEWY1KNAs05KGjvXNxilDDhENAQXrKuaik8g722%2B4LZLUUrjLZ%2B%2BHjjzaS5TO9uEIvyuJNUapy1Hyx60F%2BM9oCEtoFKOZMMG8SA9vjhmGlqbXPJpdeTjFsdgi5gTbf8jcIeJVG8tAb2AMk7XKH0APON0u7VBMgenNzls33WwvAOG5JjDgcNh5YUavnnd4ZJIcfkNbxggpGLKwKhr5uiIFWvAaG4Gyb%2FaQyaXiQI2hfUEvfB5eY1yHAaAIKhYptsfrs8DJ9vnmg2xpyeW%2FSU2l%2BMJKZEWabT9AbfQ7U9ldpH2FG9pZlt%2BLtKhyhtUUKUzo7PqpGLGQnF%2FqTEWkSegvQfW%2FyPlU31g9DwQP2Igv1jQHMKHIzsgGOqUBGFxhHAKh3We7tSB9YSYhGuF2Ghtll59Z1%2BjNhgtc%2FGcp4j29JQneIxD3Y4B4HQ8jQ4NYnjbuNdYsXXLXXVSI9z%2B2kLgNARMQzVaxNNfWBJikyGbaG%2FESbURYmdkZZU26%2BVBTDr9zSPhpr6EaLZ8lLuQNmdh4TojlS2ff8gm%2B6XA6KVSiT8s1VObmRGV4kWR3uhXK3FXa56pG%2BkwTa5OPr%2F82hEnz&X-Amz-Signature=f564deb28e48e3b095d49a2f2f98ea3c9a13cf43eb2c505a252b75dd202f1b16&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

