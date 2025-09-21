---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R3FMGM7P%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T030039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD73ZI3J1Y2%2FknmsYKNexjSgUOpX%2BprI8uH2Kg%2FvzPgkgIgdf3ykqU8B1BnufPooT6JIDJ%2BC4MRqPwbisM8d0mfVKsqiAQI%2B%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFEvwY3a2%2B4S0YYWFSrcA3wTqsDcednZoesHeqr%2FZcUw0jJ%2Fxz7%2FHgaikkbIsNta98k1TzyxVWntoby90webOObTJ3fmT2YyxywKsP4CA2%2Bsd40hWHFvWnF%2FZskHohaXOL3zzNMqvHqJk8zURYp4FTU%2FHh9XAAzdjmlcj7cZp0jBdWPZO%2FHYWpyVxRWA8LVowkqrHq56klLoVAIye0VICN23%2FZuFFmJpONs7LkVT6rdLjDkkwgiHGUJSlQ2awX1I48RTapXzHW5dT23W%2BLo2uvBmD5F6S7rC4YnpWA6Z59mv5YoUSXG8igsYoFFScobrd8n6FNzpAOcQby4G0xFb%2F6qurcj3g4a7iXOuhoIyFgSkwLXIQ7VDbh0tMJcGspTHRviHdL5rBNjXlDqjpcT%2B4GGY5iBZqCzdGN5tMH8xfwdTZlPss6EzG6CwKgyAsn0L%2BE9%2Bc8IZw0YzrqBAdfhbwe8rKKyMybIoaWG8hWDVXgngXRs1rW%2FgTFDhlD%2BFW1%2BOh5ggEj%2Fm0ihSVVYi8nCvcN2zRN7I4FiJzDiCtw8UH734LDL0Je8DO4mhvTH2ykCc%2B1JZfp35gqzlxGgn8fXKHNtyXFTgQjNtswsWHUGbKrOxFeLQUDNDsRwsD0X4aV9Ln%2Bzz1AoN7AO57W8qMPK8vcYGOqUBDT%2Bbwfy5m977AViP9DFJ3APCR8tMHmXn%2BQQIxPdYf5q24W9hko44uob0FYXrlWcFtYuJJx%2B7hOgCjIrn0cYU6RXD%2F8oWV42mjCEo1VEaegxiNdiNuIYtOXPdodX9BUVeSt33Y6gV3kOa1lByPJh7jf8M0oODnjcQGX%2B1S0ClzT%2BHlpRfi34p9NlXET0SMd1Z8xioSOd8jMcxtTHW%2FBkGv%2FBtkhsR&X-Amz-Signature=0b205348694f6f9dbad76e91377f5f1ac4ae76af0f0f9094b664dc289f29980a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

