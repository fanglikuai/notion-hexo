---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VF32OY3B%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T090046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHwaCXVzLXdlc3QtMiJIMEYCIQD66ChR81H1HCpFATNveJQUXIwuvOM0Q0imYfgCmAMk9wIhANXbFKM8tHTA4wTTjWWM%2BNGmh50zAfvEMV64nWgrMxMfKv8DCCUQABoMNjM3NDIzMTgzODA1IgwNe32jIFBL0wLmZVkq3AO639MWJfPfVbqw7WBB%2BxRozJuXcq6h%2F0wynlITeU78HhAUG4H02haTCPRNrNWU7Rl%2FksMsIKNjjAuD2bsVW5coy6zhW38ID7ljhRGLkQl%2FzJQy4sRInU0Yz7gvMDHRD09U94MHtBIfwcFexwzeo59bgElnK%2B6G2wOzDP5%2BRqQbqJWveULKuk51ptHOjoFHv%2FR5LLkhXoUcKgE%2BDNQjJYbSc44ikXdeH0z%2BSkBccOokffSxvMzXnhugU8YvpVDWFEksG4TJxCp3khdMjxNSmUQZi0yLk1Fg4V7meANfVIIo%2BbLOqUMzp9Y20dq3ETgsZDXnjvdWeQZTS4a7fDI0lvrB1%2BuHmFSbhaKUr65%2BwFIbzxpJV%2FESKCUMh7CDKzI65Fv%2FVjzyX14MHYtSS2zKnsx5pRX5kBJHBZcYueRM6ecSEwlOAbbMD%2B5i5L0DunKzLV9o0It2avBNPPs3qxfi%2BH7gxFKkzXsGOZ6VRRzQ7CsNPFdajDgLhuApCsa1tHPqEQKVgCy3eECxLWPU3bDgSiAmVareaMutXyXS2yPIJLk7R31olahkdPFS0Fk0JBbNjkG%2FVcVcyL5eCi2vzrQBUlt79q%2Fvw67g4B93DUJmQK6gLtTpEZAlPsllQ21JZTDOy6zHBjqkAcgJH2AP4ZGx2l5X2OQZBsw2vy46x3DA6b5fsWP1%2FnvAx4Od75b2NK2jdxszK7HRY65vQsJg5%2ByBneJv3EwDCJ2yi4HRIc9erduQuXSwUguFY8MIDcUTMEpf1yhSt6rokIWeAIMJeXr5ax2XYwyXO70O99oR44ylZP5QgQx8HlG90NqeMPao4I%2Bo7kgg3LkK1vLlFDJQbK9KqGkQHOjIMlIoxlYF&X-Amz-Signature=838d7ab744c51ea69ed3ce80ead8b067715c35ee3ca070c672cf0c85cb6dcd09&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

