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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WD5GFKYQ%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T040042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEEaCXVzLXdlc3QtMiJGMEQCIApZxBeL%2BCmAhPSaSL2DdCGNUWTey7HWK32LuJapa2b9AiBjbrlspTzqNZT7Xyo0NfvnESPtZ68HfH%2BQBpzQsTu5EyqIBAjK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM0IMDIEu%2FN2GjFvfRKtwDK9OY24344QwKbVpxlkfLc0VoRcqiBbOJ74jeaPcYqfVGolcep89NiseVb%2F7rrr44Fp%2FQi3KG2LBrTlA8u9dknDzdfdyfTxQuZglZpP002lozKjX7QqLVSUnqhht9MmJP%2Bmhz4M5YgPtQXzE%2FmxVuaoXr%2FyUj5PzeyCz%2BkJWpvk5s0p3M5bx9sr0PAISHkfjQmBFgvUSB3HzV1P62dhw9uvmMptDpyRVrd0d%2F%2Bved5yxIFGLL2eVlqif9dCHhtq0mJEfBX2bbQtIAsoVSc2qLqwDJuN8Jq63RN%2BcwlYvONSrrX70aJl7Wc9xqgnWncDLOviDwwRY2A%2Fsr%2BCvKAtpAmV5ik%2B9F9OdFwnQivXOuPZDAokhRvU0%2F%2B8TujDYPVcxoO9LXsA%2BuTTO3%2FKT0wQfLdNmpHqsowkzb4hlGVk%2Fq7nKsHipvmKTverUJBwViRf8iG9c6ajTWUeOA1NZ%2BPOozeiVzZKJd1heWKuLeGFf45G4ABhWm8pNHsZycuLHimmlm%2FNR%2F%2BitLaImWxavP5q1s02PCEneI2xgtJg3usaQD%2BCQfXov5nPGR40tXrko%2F%2FAV9BNnJz3MwyycfUK19i%2BmvbwH%2Fl4Cmd%2F%2F7XeyoTq65Sfjfj6D9ky8BoK6NYVMw2qvnxgY6pgGeY9MZowIVvSxgZ7HmDLnGETXvgZF3GwwDrGZWNNMkEbeW1xefL9ueMQXkkZHkJtg0CJoS8qKLzqihH%2BOFr6I1tEEUBM65uSyk1eYvqwmFneRBdnpTVQfY2hce2ynewGCCCUEtzSaRhHz23nGn8rhBnRHZHgmz8a0%2BeyNsk0WD72VGmVDrtLy%2Blj8JY5WpwYrEqth3LCTpWVLO2eA%2FkY5unmAiUog5&X-Amz-Signature=2434db1eae57507ae92531e8d2ab514cbfd53137aeed273aaa188d7b1f7a4157&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

