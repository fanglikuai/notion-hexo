---
categories: 整理输出
tags:
  - 分布式
  - mysql
sticky: ''
description: ''
permalink: ''
title: MQ解析与最佳实践
date: '2025-09-04 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SCUYUZWO%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T220039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICic%2FwWBrsqbRavqkyUuUsCLAaRolr4FBuAeflOjoBXVAiASw3kZPcrX91C2tVTbNyhOa9eSoXTWVrC84XBLdRTuhyr%2FAwhmEAAaDDYzNzQyMzE4MzgwNSIMv%2FSzRIdpiWxaVEePKtwDSl4TkrHcYUZXZbByMNh96tB0k%2BzpjnH7vdBn13gXvvzBtSkR7SGHD4hYUbFj8oi6QgwLQOjibiBWYlf45lX1j7xAEdHdeBu8fPi3QFStA2dVlbGcZEhPtCQcFQ4OpmqT3EQ%2B7NurhqBuH%2BuhUKZIDN1l0VKr8R82kpajnE%2BGvmFytsdv17LIfzm7C8QaklN0mKHdNNpmvJL8tP%2Bm577rycOeQcr8dDNblmQwkI6vzjAQ3aztBR3rEEm8Kf16yWw9Xc4oXNsze%2F7e3nwWQh7ecAQDoE5ocODqxuf7Llq5RaQcHkE1To9a1QSwKxOAdyJLzvwXYe0aqlzeq5GhIuIWgQFIYCK1%2Fi02%2BZvCTmCdpDftPtX5iKxvBnLXxqmUHFQ2pv41RYeUZCTAM%2FK5Zb2cSOJplxctP2MvAPoOE1lYdX3gBmY%2FyA9LUNsmHiWHl0qpxQyplwnVFaGCqmTSgJhJqeS3m9F4YxpGRUWqw2NCeZs4HuWXKDJSRmDeLnKjoeDwDiA3c7%2F5k89tX%2F5UNfauBknv%2FfVRWC3j%2F6e%2FjXcgIAydCKm8Nb68q6x3YBAvoC%2Fdrx21ZB8Onupl%2BhDTnms%2BTZdKYrBNJ4DdgbBCO5jqYv54YbWukyjMnu2Zo30w28LRxgY6pgFxyNYZ6QOiIQu4uC1hw86wam9dUXOXx%2Fl0Zhtr8uqpglBV6Yi1FmAZG%2Ft7ekJ5k12lvspxSdlL0WFjpZqBGq2BCIzH5qy7unzPD%2FZawHjkIt6qRgdOaZEM2nFn4r%2Bholq2FsDFWRujjI8sryQct5n3WLYwnmpYBkS%2BZqzfMole5CEWRuu8dwZTFZDWHptvudUh72tRTPFthzVgtoddWj6PL8nTabnA&X-Amz-Signature=34492022420edfa6937703836b8521e43b51f68c4f31198a86daa1578737f527&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:56:00'
index_img: /images/c8962001455e38177108499d1e1e605b.jpg
banner_img: /images/c8962001455e38177108499d1e1e605b.jpg
---

# 丢失消息


## 生产者丢失消息


使用ack机制


异步监听：

1. 成功/未成功发送到交换机可以触发一个confirm-type监听
2. 交换机发送到队列会有一个publish-returns监听

**但是一般不这么勇，成本太高，丢失概率低。一般都是采用日志/邮件记录，手动维护。**


如果使用定时任务那些，成本太高


## 消费者


手动ack


应对：

1. 消费者失败后，将这个消息存到redis，记录消费次数，失败三次就直接丢弃消息，记录到日志数据库中
2. 直接false，记录日志，发送邮件等待开发手动处理
3. 不启动ack，使用springboot自带的消息重试机制

# 幂等性问题


原因：生产者没有收到mq发来的确认，后面本地定时任务把错误日志中的消息又重新投递了一遍


解决：


redis中增加唯一id


# 顺序性问题


使用一定的策略，如取hash值等

