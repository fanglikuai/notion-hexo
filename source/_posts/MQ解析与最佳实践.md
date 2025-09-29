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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WYFTRCTK%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T160102Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJGMEQCIAQyVZPJdXive9E3oQRpJtsO65k3NLdFX7GR8avdDio%2BAiB6kyW8imm979xNM%2B%2FcqXg2tU%2FjxIeCI8DC%2Bbn9pQMHUyqIBAjZ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMkuUeTPR11t5PzSTeKtwDXi8snyu76e1N2o0OF5V%2F%2Fjdt6Y%2FAmIfBdoBv5kBXmbftqdyo9xSyD%2BG2G3n5QoIKeWp%2FLY5HiD2e2t7E8hdCtNPnMEYEohABjIl4HVVoQyI8tJSmr%2BSjoGQEgpdjs0Elr52Ba1hN4tM2gfPE%2F8r3WTPngldFEuC2KznA3q3kJIxJk1WZJHJS2wsIf1y9SEbyxnZrUrCzU87sNAcgS%2FzyZw4jya1pBCp4Ok%2BU84OzjhgnapJLaJKDSfeRGudjK0nAU%2B82S1S%2B0jhOyJ97Z0ZBRQAJcLwky7jf2iu2B6xmxZb%2BZxI7bzvXRsmRzKQcRXFS95eqcmtRXtHO02ft8CJGU8vXNW8ENoU8qreKlM0cDyeSfFbvsJbY7y4DuU1gMj2E3BPP35ywj9vry%2F7mHMUqsX21gOK7Q4AC1mmoQ1uN%2B40m4QfIZ4KqEeekGFdC%2FmD9jwfb5wKrDNbjO2Of0NdfZ2sSkja8lpe5VV7ymrHsBxvF2p625G9QagueteWvIgWPDJEptIN057SmeZU%2FOJp%2BDug4snawt1KkO75xO%2Fy%2BlFS54QbnR2u1c1TwD1ZiqOb8VbXWTyY6Pn%2BUKOyopSiYspfU78ylr2pc%2F6zf0keD%2BxCEAJ%2BclPmBaQk9bFMwjNXqxgY6pgHFaxatAtIzkdzls7nkq%2Bl0QSKkNqfdFefVSeF0RsarG7PIgCs20q6F2Vo%2B20EW%2FPdwNtY33LHvGcusFDhKdFduhiz8geyGC6N83%2F7DA9f%2FNdXmAYKDDaXJEexBD2uvrne8X2c0zmEqY54CGa6WIYH%2FgIT9D4hSXtxwlknr73QHq0oChwK4AXQvnMCxTtKMkME8gazwqAvDDliqDPUz4j9zgA%2BUab9n&X-Amz-Signature=c343234f36a97a6ae1a34a324f0a0922063f0126a9d5c112dd478a83678541c1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

