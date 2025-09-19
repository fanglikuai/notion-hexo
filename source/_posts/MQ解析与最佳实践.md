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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662RQNLPAJ%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T130112Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFoaCXVzLXdlc3QtMiJGMEQCIAuJbb8DJHBsoFmfJUAq78zWHMX3B1F2h1LdEDMy4GY5AiBGWQCKcJDPLnV0zd%2Bc2KnuVTq%2FSyFMpd3SqMjtuZR2sCqIBAjT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2BxkJW7m8szvlTL%2BnKtwDBRiQf1FXz3ueW5R%2BFL4SRtlR7CB3vqrffR%2BXQHRI%2B1YjD7Kok6JtRtUbdzkdQpYjpe44ujIXmTrsQZWeS8%2B%2Bkcs4uHn8XdUdF9ZA4DD8lRGgho9NLdpQR2bTjGK2an2d%2FRThLm1r%2BHZAG85HV20KhBF%2FDaHsQsMctPh7PMPFUzTLTLQLEbRE1Fli1FrdnbWKhf5cLFCGwZreiItK7lkTNw76K6FQ6BUUjE4mh8Rqqc%2BQGriiaAHRfPHGfXC%2BoyoHdYN4NKiw05kWaVSO0D5kt0cYSt5DaKrEeQBRtA4b9XRLrkG0j4PZlQ5%2FReVSBSBeXpVCYR9UY3x2NczJ85JD0AoifgX3k6CXvL9K%2BJ%2BiUaXHBGeFerFL%2FWEpi5KD0dJFoZh6n6Vdn5UelW4Mq5tXe69TfSsNEKtDXKn7EjZI4jdSfD3YpMNApmrr4AUEFzhohEXGRP87YOp09cbaBbNg76cQAuXvXkRVshseb%2Fa7V4AhVoJJ0DKGK4gJHCNqXYbd%2B239enpfzXik00ZQTGKe6j0i3urd%2FhZzHxdmXHXjR2KH0AJoVnGo13Puj8Y1c5UQqpNLiLGyL8xUacrEP%2FhF390rN%2FUV5vvfFG1JNNOjSWX80oxiVTL1wJcu%2B7Iwl9K0xgY6pgEJQ8vx0EmEU4G0CQgXenE0RCtlzHwoxZibIcRd8dZA%2BnbXA5x47h8tQyYKXNG%2BcZauniNyCcOR7CoUcER0vcPm8pbzCGVxlO%2BIDaV%2FXZ12jN%2FfPwXXo%2BUJ8s0hK1Q03J%2FQk9fJORov%2BMTVFuqBgicX1XIiv%2BPjlYABY%2BZ6v7WKLzq1RAIZQ997S9Ouw5oWvtKkmjlbK8F9Ye7P%2BvP1TWjZo8t6dbNm&X-Amz-Signature=e1a552baaa70bfc56d99b7545cadd736a4d26b8d174bcbdf400df2ea1807095c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

