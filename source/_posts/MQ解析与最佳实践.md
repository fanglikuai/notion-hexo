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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VDHHP6XR%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T200048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHQaCXVzLXdlc3QtMiJIMEYCIQC6r7evkeIUzedqR1Nnoei0XblccfcD19aX6CaBV8cPqAIhAKBz7SmbFEmlj%2BBwlvEwdR6cmWrneC9a8afN3xYidtnRKv8DCD0QABoMNjM3NDIzMTgzODA1Igxj59NF8jyGYwReNDIq3AOrwMJpUAvstZBv6WyXT6glV5o5UozVomO2BLKrlVcMnzZKaZNA%2Fp8PVG15PJoeUY79ryHGPN4M8jabZIAiLw%2FNe0X1a4h7RMsDUpUnC4%2F68OXrWvwi2ue19u52zqiWm7Hi16F4s4f2HeaHWKZjAFRdDQG%2FShxEi%2BT8tkvn2jpFmkyJBIDGLOjuWPwBgMKuXAXJFmr5dSg3LlKFz%2BWvH5%2Bxzc3dwVZcgvRoA%2Fro60pICvjf3v1mRrxfw33UGmi7wAJrNTxc9GP9U0DsSdBlGBRhdd6NiZXZMw90rogkdxqI7UdOB5YBOVwqGpMWv1Ii%2B30w%2F0fdQUyWMW5gOMTX2zK2HsRjYTsI2gShERbps9WsTyPjS0z11vG%2FlHPE0qtAfoemUyYF8KNJ1dxOOFSgY%2BL80HzRIRic2JWx9qTxelprtSwO%2Bx340W2rMKbB4vP1nFWBaGHzvbbms0WUWNVut7%2BEfQ%2B%2Fvo7T%2F5qola2L%2Fp827PgC7Fzt2Qn77SJM%2FVSnglpWPH0J6jdkxtQS4pzJU40Tr0wyq4Ya8yPUdYDHRyIAD2aZ1jqTFbVasNcfBtKy4%2B%2BtbAADq73BGlsr%2Fn5RUsWtnAYhm5ebWCzAL7VEvWVHicZ5WH9vhhtjs0po1TDnwdPIBjqkAaFIbkrlF%2F6kdny%2BGVbXopGZkBoOG8UD8%2BD05sE5AygLHCblxCQ6oeZleXBM%2BsA2UavsI2oTVOOB4%2BhO%2FpaBYBj5THMzG%2F3IvYQW9EErymwm69Iz%2B7%2FyPugJTVsaAKG0n9qFRLrHoj7F0jy3S4btq6rWdj1SMH03n3jMXZN4aY2MPDmpd0ZqhI%2FrFq3rRK%2BZung72B3MUtZfRt5zPzxjkALWSXjz&X-Amz-Signature=ecdd29be541a034ce3b0a98f92c97b2dcf4ae34d0c576512009d13faed6108b6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

