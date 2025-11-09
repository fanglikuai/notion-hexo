---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QDBANFEY%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T220036Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJIMEYCIQDiyAuXe9AIZVHoMgY%2BL7KUBmeS4Z0r0c7Dt6ppC1DFmQIhAJD%2B9pzaVXglPw5PC%2FFCgMtxhoG0%2FMQP2FETT1HuYlxmKogECPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyHqkNV%2B0xaLW6ItrYq3AOJNZ3uQLGqvSrJKp7o7nsyLRQ3fS47fDr%2BV5hVM1hDrkQHKdFV0jyGJUnrPZup6%2BohsAngUnIKN8hP2PPqHHlUtdQbk9tKe39eoOc0BXj%2BINJUuhUULxciKbsPE0DEbVGOBSKhvNWXJ00Qtet2JoH4jcwXRdwvSozHC6DmOcF0pObn45%2FTrsXppavnVk09Y2TYTZcOznt%2F5OL%2B2slhsjBo3kG2vPCoE4OxIWfo%2B0HowvYEewcEE2OAEEgooVD695CHdDjWlDsh883vn1KQPdwSXpf6l8myMjawWLcI8MFC4bNXVum3Gcp3D3%2Biju5%2BgR0dDhinHVTY0kzAPMedqEiH9H26MVL7bowUIPiY0yaTq34gCI1DaVslD9XZDN%2BQCG4r%2FLLCNBxaS3P%2Bu%2B%2Fv033pnhZEaXW6keIpmIZcJriABUWXtOLtHrRgezyqUx0jqp%2FJlZQ7qYM0qXLO2VekXKsVMkvL7PXw0kVsYIcBsAPXJAn2np2MLvi49axRqddyNIKpwEH6Z%2BW%2FxaDCgQFnrWGO1F95hsuIPuV4pXvrUJV7C3wjj5fpRJlswBdLGjhVLEvdwj3XUoxKruX4nsZgIsfQGMh1kVnlDvLaiMG4ZxnWNWotRDKsL0f%2BvcjpDzDPgMPIBjqkAejCF4iD%2BwJQ%2FJsV8iAC7XKO%2Fj9cEdXAb4t3krfYN%2F%2Bz2zxWV5XrljwxKCRYHei%2BL04SaulRPeWZYukt8zSypkbVXqe%2FLPTg6iJfzRFYTqZvgYrnNP0hXnVwV6JmPvDuLuZ8UTZ7Ihy5uecVNz%2BnjbWD2LjaLH35bbabck48fHhJeHYgae4TwePnzQfguM%2Fb2jczOk6RUUQrI5YERdwPdeNCnVw2&X-Amz-Signature=f1db288e2e3118e2b950ce8cdc54d11a381a80341c0a2de0c368cf40de6d4797&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

