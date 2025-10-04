---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46627HB6MT6%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T200045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDRCXQYcQGWBQoQr%2FiMvkgolBNozJocGhf1vHFCls01%2FQIhAOqJmhVSxjMPUWsmGRKBsg%2FJzN4GZ481gkwLWnvM03mRKv8DCGEQABoMNjM3NDIzMTgzODA1IgxQMCe3X%2F%2Bi2Fuit5gq3AN5ZXz0DovJY9O8gmQEhb%2Fk3RCcvBLyxS%2BkNtNNojmwVvk0kbSeG0sx%2FN3FKkh0fAQufFzD9csiC%2Fzm%2FpfR6wVVfpUkeBESqSXaD7c3Yhwhw5iFop6wPuZeSXy%2FMejJEoNQCmp%2F4EjMsuAaZ9xfLi755kV8%2FIoypGYFfn6Qn0oS3%2FPEvqFngXipco%2FrDICS5erd5%2F0mPw%2FHD787xhHEGcAydQ6mb5I9kj8QunREsMnQuBE2CVjvVYr8JylIWB9kbQ8Rd63xwYoUKnG9iQmAN4ExRFgwVbj3RVJ4vUXrGaF0IVSXoX%2Fyn4WTuEI5fQyi0Ml%2FwbVubb%2BRukbTzFG0DkYAPaq1ITN2M8TnCm%2F8qKyYRjqVDWNvIG3s9zXTzt3bn8JvqrYys8yyASg4vdAqQJNIouUE3ZmFOAbXquupOU5gRIJb2AgoYoOhHFKe%2F5RibTgQASF1RQFIz%2BVducDAkmA0Or6kgUNnWxhpu9xX3JvdDxbRUwj1YC%2FCIECUWtcHrOZTfTq2dJOidQCmSCeRx7L%2BHmeC03C5OI3c6QRrLJJaP9gRgu0K7rMUJ4den4jvKl69FPNed%2F9KBHoG7n%2BkTfUB1ZkM1bPtMVyPfDEImNz9let3aRvQ%2FVsv7d04GDCUj4XHBjqkAdCoxUreWMg5fNCF%2B5rsDERfNLnl1m04SklLwTHMhIttlFDC44FsqjAzxsYGAi1b43uIZOU8p5vLjLrbrO%2BkTZz%2Bb%2FXlmO5bX4n1MNd6vh0jgvwk2kTFdasDhtlfMLsJiRYg6jVkQBQ8%2F3%2FC%2B%2Fae48q27GnW7zm4cVzc4RlYwgbWzQhmS3mwxrEWGBm5ngeJWwcgS5%2F30ouIKt8gcLEwQsPY6g1B&X-Amz-Signature=47abdb9f17f588acbb38cdf08a84b674b0713de8e66162a8a4f4c016cdb88912&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

