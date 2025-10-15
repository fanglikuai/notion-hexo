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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QASERU2Q%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T060055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAx0gNpaOqTTbtb%2Bev1beewfJp5u3Mhv3uDDifXsrNGeAiAXHP1YeImz8WZG%2FgCCpFP%2F8e8KlZYpi8zq7SLNSQh1yyr%2FAwhrEAAaDDYzNzQyMzE4MzgwNSIMDtYTI0yGHjmCDgrtKtwDcCWDQEBKRzE43MNpaq4Dr6oAp1RQni5MpTBdHK50UWNJ%2FnB525%2BSzZY%2FOBo2hO7qr32zggThjLbTr2vbFruafbdEg%2B5NnV81Xh83dQutmpDxvv1Ac2KuP8EtCrniw49kJWvsjrTbsiwjR24oRXFP%2Fe2KOhcs%2FJi%2BNB7y1Uzuu6oQdlPTliCP0Eko7G%2BCmFKc%2FqKyWVvoYENI2C1oxoG0Ru0H6LN59T0itOq29Burt4nlbNFzYm2tjHLtU1AuaKtKa8vKPjEEjQTTxYt5yq8G367k7AWyHRbIDssH9Pn4KhXcyKv86DrrCPdxSJhgiB4TYtsaVmhhWraO8G5ekcUf2LCcA4f3pXGmie2D%2FrgdRLIjZsNO3ao43hOkMuouX11SHqY%2BDpei9LqtuaJ27QYxRrBgrYfmodiU6BPdkrWl6Y8IAb2Ct5CaNI%2Ft6oFYgSZT%2FThFCCSf9BH9chErRkVlcPly5dUmEG0TJ3SNen1c%2BlBu1wPZIWmR7YcjY3vKKgIBIJNYgDwz7s4B9zm7d0%2F0XlAc%2F9Zg2HyE5rgfWVaW7Ius6cWzuwiIk0uqfmI8730XtwDUUt5XNs7hpGMYMqKHvSNeHnrUN55CG5lKMsBKiuQu1IaKGCjc0lGTgaIwi4m8xwY6pgE9Ue29pYdhDAyyhGRhDmajntdAw5VGwA1kly9KKmH7JWayzEgmwhq7CJa9SUf4N6gLtA33l4HZF3mWcWx8877U2U3HCU4V573FnCCRd7x2wUXLf5rVH9zIicfzAVS7i9VLdATrwg%2BKU92jO8kczpmb37e%2BWhwuJpkBZHhFJyDwz0OL83XS6rON2GXEDPnNMDeU3LpZSIDDiSuP8QxihteMJLbIMboe&X-Amz-Signature=283ec1e55f1bb2d2ee536e307acbb6953e89f71acfe259d4077dd463b1828606&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

