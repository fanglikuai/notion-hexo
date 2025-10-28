---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SNBYOSXC%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T020053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDIXI8otClm5WSPLbVuE%2FNfd4VEddDmYvNE9PRe3RFtswIhAJxPoM2l0xfm2yNZFHxXwc1R6fXf0vSmPqMjieQwUzCvKogECLP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwdb%2B0p0GHG1kEUUZMq3APvOzLLyRYVNHstvJ%2F2JnyBLklVK4%2BHj6a32aNzqrcnHOd2Q2kbQK2W5UQzZq5ByixuX%2B8PtXrJLw1gjX1Qvpm%2F%2FQcLkEKz1vo1PpSga4oLtKtqsOj5izjfyhH57La1XXhC86PMkMKy1JXk3%2BFt8N%2FSTJHBr7J%2BKimnxs6cDrgoW6qU648phbWQsL8r5gPk%2FL1ee06DK0p46MVNDsqCgNgF%2BJXDHTSpJ0%2F8yhiWBpIFwl3OpySBjqPZv5QG7xc4JCIrHyfr055FDE7a%2B%2BiExddDYF%2BfoknFr0pTaS0cnkSNTSRUAo%2FptYt2wAnwI25I9cvMc0V%2BZESrsjBWcsfNLam8cfM0PP7l9IfrqYmD2IRNlgbx%2BVeWjUUYS7w74KBrfp1KjM9RGqiqOKCu3GLE0Uxf1IGnOP3KnJPR3anN3RZRXTxS%2BdcdMjO5vYWylzRh6Q5DZODh8ehftRQgg4sUJ9tYJSaxDDMbVHPeKH4MSkO6w4dhyLgzAmBW%2F%2BHagKV6V2CVodSsnHBtbdhxo9kTNJnuIL9T5wjNk0XE2PzBZXItsfqwVcVc2%2BqayfP%2FLEFPZ5xtSWjS4WGt9JcXspkD%2BzaprHF%2BEoXZ46gWJu7CtuxyTt6EiWmPZDF4t2L0ezChvYDIBjqkAbcG%2BB27RzJ9o7t5tQ%2B%2FzFYgzPbp%2BM2A%2B3jcwND177nr0d4867LwzD0hlQyk1D1XXJdRCT9uC5orHqrVOelKLLdtZWhZPWbtQq1PB6SEYDWFEq8lcHvOgUIP%2FfdLX2kZs3s5bQp%2ByDNQ6RrNI7nvUA7B05pY8VKrSvUUiZjwpMXwioF6g42s91laFXof8SOYkyTQ%2FOsDMbCCx5mqfcCeGvDWdKjU&X-Amz-Signature=0ad1c68afc1a66a21771058bda646ba5118c7726a9b30e308500c5fd7fb71ccb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

