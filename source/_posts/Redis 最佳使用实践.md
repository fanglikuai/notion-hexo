---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666ZAOOVG2%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T000047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED8aCXVzLXdlc3QtMiJIMEYCIQCG5lLmMBIVzdDLFEiV1VTXTGt87%2B9knWgUYcHhOGeUnAIhAJepOpvUBudsZ5ygf%2FLyupGYSCBa8cD3BXUPlzncze%2B2KogECMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw6l0aQn7rSXx43fJMq3AMoHlFe0Bw7bVdsnBIvBamCkbMJo3oKMFRuZj1sa1lPok99zWox2Kk7U0zgRmc6U27mLWRFxvbWLyjBAmE8opFQhXpHeUSFXtUrdJj0XHvEHDr28XHI4a0SxRArmwTslOeYT%2F6f%2BVZhv%2BDfYGDxeXhfy1DuZ5Z6gdkUau4jpdzQ8Mrb2GOGQNY%2F6e0vXCWLIH%2Fvsort1mNFPJ2O71z9JMN9918Cy7hFU%2FAlUY09SPavI9jpvCV3PG8ZgHx2HiPLRJUWGLWv1NNho58kTNzPWj9wMLxQKFG6W%2FzLaXCzA%2F1AwtNBw9AHZU98oueMyUHT7oGDW4Nce7LnQbGJRxMDtSqMvVWLBf1oc7tLzWUtY3ajBg4fir5EfgMzM3Uyy538fX1OkyyO9D1wSgYLjvbP0TnDCWjsfwdnHwjSQo7TheVhnqNchKRGTUGxRss7wROZjRZybkQgDe9oyoN65Ym7OuRYtycQLcdfVnvageoQK%2BMw%2BHM%2F5%2FDejdnz9svllxtSb2wG1eU4LySJx8sGag326LfPRg1N3gdvVeVNZxUraZa4SqIOTmSVeOYSlwfH9XoKhykiEXdW20VZPqhSFxuoTfXXXGqK6mZZAOV8QUPHiP%2BSZWk5Lk%2FKzyUxGAY%2FRTChgufGBjqkAcAjxtT9JrTZMZwSWzL7vcrsKEru5YWe9UmNSNrayje3CzgcwaarVHYaJWQxUiP4PRuIo4UGUVf1W%2Fy1pE7JTO1ruLFva0hROik7IOzJcMBYot8PdtUEqPXdCY77Vqo85vFwEhhBwY3PmC0gSokf187s8SCfwzw4YDUcUroc%2Bkd%2BxF%2Be5ndafg%2FhQKd1NATnWo8GdpPjhez9ALehGMEUyTZO7tn1&X-Amz-Signature=16dce1294608bcd12360bfc43e4765dfd91abb301fc72ba90a32192c4e6bc858&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

