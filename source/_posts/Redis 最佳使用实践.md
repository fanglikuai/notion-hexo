---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WIFQCEQL%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T030046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEMaCXVzLXdlc3QtMiJHMEUCIQDAyKWJ84JBJRdAqdtsgF2oxx1BB5HIIAuSoLXuTDvFcAIgffjbjbfADQI7sV0WXCRWystKewRE09lMaWSd%2F%2F0Y248qiAQI%2FP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFOESXv3sbEmtbWZkyrcA6DUVfVE9TGd0l%2BD3ZLXBGISiI2az0DlQnsPzNyk36v%2FBAdR%2FXyKSnqovcCVrg6cA2nAtKqk%2BEKtZ8lKbJVIUee7HsmH6QoyHY7NYt88pvy%2B%2FIFvGtENAlDBydXbWrt628QK4OnwjbQhnWgK5BX1tSOyDcwEgeQS5j2usNAevJEyIx4mHFlLDaQHmRUzaT9hTm1i0M9SjNQnV6ehCIfcDO4ELrnzecgttwPxq4JI34hK8iweBC7SQRhCnibpVU20ypLD8oJ2gXIzRMZd8nKNV7rbVdE8HXMXJ7mn%2BKSxgLezHdDFLxWj7n%2BIjXNtKdW8%2Bql7EXbcB8frH8cRmua8IzYBOnzzW2DfwCrudAG17PJBsHO58YFymsx4Dg0fazifhcHsrlpztArm3sSLpbopgPyoKLmuYbF8iLoAtC4KIKn79gB%2BOnWJAkEd040Hx4cZ%2B40MoSN9g9q02gRg%2FhDgmRRR7LZ9hrmI6wym%2B%2BhgHmcbfz44DqHZUrmxSvqX43Piq4NndUDliezBMsDpfW%2Fy4EzzqZfN7L54jTezfS%2BJ11ii0bKs%2FoT4CiqyiViUfanByLdgpNY1DKN0Hqa9X0C%2FiAUFFtAf6v2kioyFXsH4Wx6igX69s8TYMGaTgtyPMJjKkMgGOqUBAYrLC81yhwttkWk68vZXMo1M3RnF99Airupsr6fkP8RmvCLApk0%2Ba356dLM0HU%2FhM%2BwJ79LrrUhrtVM1d8UZvW5guKAgMt7CMDCh0QW9YJLzdX9R8nYnGBOyjYU0pud%2BsSlXxMGRlugEEX1HRpnem7xMoph63w%2FW1t6LguswsSbTttj9gseqLcJVbseMBZuGKk58uRwTPaH7oU%2F5oidvF58ch29V&X-Amz-Signature=962c9cc8e258f8217d3236ed86bbc24b13ba2cfb54c43b2172d0a0a153833e11&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

