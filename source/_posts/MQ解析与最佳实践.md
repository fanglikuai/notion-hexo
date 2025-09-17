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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZKYLH5U7%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T130103Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC0aCXVzLXdlc3QtMiJHMEUCIQDKtIE2nDcuxdOI6ogLCAOsBTndUR0dSXO5hWmQNAx8vAIgfqbXYK59RZKcY0UOS%2BnyE267XvgXs5TkQu9xWdOClgwqiAQIpv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJW%2B3XwiZOsK70CKOSrcA8H8VD1VibYHSIh419U3IYpq27HfZSPuiqMTnpokWdPTB4yHlXCwsATR8LqvyBu7nk%2BKnFAksfHd3iTdxXO2Igv88zPF7kss97IL47MiJMpIcdwV4j1080DO4r%2BC5dCoy%2BLTlyviRgjJ5%2FolSatUq68tbGxzDixH3FyvDb7Kagvcgw28Unse%2B6aQ4rDZoT%2BxYFWjHcIEQyoAkXORq%2Be7xBk6lrbZMJa1p3seXuIRWBfOr%2BxDrXKxQimhlWG7lmsBH2OtsnK37192r2dNaQ%2Fzevvp9yH1CAfbotg6dt1yibcNXOBk9f9uhPjig9qgiDmfmMscIcRkRxiOWxXug6XBumm052F%2FzMQKZGYXVowJLpaKhj48PIbvNckganbY9y1NT4EBaXgEmxeGguZRL96gw%2BTnjL9s8v6HSJUbJssNzGjE%2FAyPmyuv4yN6MD6Qvgw0TEQ6SVgowC8eZ1VHZklMdVKwElcsnG%2F%2FhPz9JApI8rUbg644gGrgsJKwrTYw9pK0c%2Fjc0v%2Bxc3HXR9crSIgtBIy4MFe5X43zNT9UByyMHnqtbRVHqRmxFsBxUiVINV%2BFzVv2Zq%2FrkrgW8nKVlFpVFnPcyHficgDb4JxOdfPzWpH14pxVjCQIwO2Hur1UMJTVqsYGOqUB0KENV4u7fW3Bswh323xaHte4cTq%2FRJDz19TGHzLt3bN3JpjV2UN1uKYuPNfFUOm60vCq%2F%2Bw777MrUnK%2BqDYjP6v1Ti1Pi%2BU7pWa8P2s44QRRzzyIieeDYEKOVfb8RaWvEb6aQzTX0lTtIdL6THlAQPBCUPnSFTPmjBLWo3LrfpofzGaIFtqCN6p27O2yfgyTd%2BxP4s0J3ui%2BywdZGgqyXbSMG8xJ&X-Amz-Signature=e7de4651f84c9c88c6b1edb417592486d89aab69d4c35343d20599fd52b3ccaf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

