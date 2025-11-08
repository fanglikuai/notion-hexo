---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662CVH57MM%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T180047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBIaCXVzLXdlc3QtMiJIMEYCIQChWiDYVX2Fus9lcbLQFO1eMAvMQhfhM6bjWfYfZ4JRGAIhAIOWCVOQY4snrhnutUO4%2FVx0NaW66MnlQdGoM08%2FzqCaKogECNv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyEDtGVp4nti21zpOEq3AMZ09Cp4%2FWn6rno7HTS7%2Bx5gici5oxB8KlEy%2BpsTguOD12qHKpyoQ7qBl%2BT1ldy7NNgFziZQDTtIIo5pFxGdx%2BScrS0SvgQQpusjbbGD1QPJYQF4JS6nkwHapDwnu4QfY2wq%2BkXkcG4fkZ1lqBprHdd6iyHiJvuQw5HnlDOKZZSm3zWToenj97SCI3ndaCfEMNyn5zTqvLckgho%2BXVsXKIqWKpMQYrjIvGz224j03MSpH9B%2F4aLWuBEhHtbsPFIvw7piO1Nz1pvfbFxzlCvZqvA%2ByfogCcArZsHNyPXi%2BBIUjMx3StYPwJAVIl1a5H6FueVMQpXfyGz7oMnKGdaiadmhXK0bI6E7h100geD%2F2OF0Kp0JMdS4ftdV%2Fqu3rUFJ17OtSw1btD2vppysJLS8zuu4OGHbX3rOTbYkj5hRC99ObyLFvw8n0xM%2B%2FSxn0vkZFTSNzt%2BYAdtwnl%2FjIpu9tMSAx%2FV9LaT%2Fb5%2FFZkixpWZ58R9QAWr%2FR2ALpIve4Ffzct4LL6T3UBjt%2FLKkjz6Z1xboj%2BGVPhTue21cphBEKyNEs9BaNpNZHr%2BmdGurmjKtuTwK%2BylmmhhIqg%2BXODx7cHWUnRepWGSOr%2BH%2B4LS6fRlFeJdFRczgiaWPWFUxDD2%2FL3IBjqkAVRHE%2FdtcoWpOLSHTTFtj%2FVo4gD53BSUkf2qSSmtExXvkWQyHWEOCd8463QZHcTvHNO4hemF%2FO47fvpO2jsshF6XOH%2F5U1voZ3SqLjljL4pmqIMSbFopeAb2CeXmwbQcRlUlPS8z5Bluk57ALy9f4x4pcyUECqd8DvMVADjkHqXSy%2B6BH8EpWVspHttyV7UpWcbGjEo9HydLVOEXyoXDDJ4XyjQw&X-Amz-Signature=4da6cd38bd093520170f83f5f9a40d4b5cb309db1b9ee5ada67d651ce02be0dc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

