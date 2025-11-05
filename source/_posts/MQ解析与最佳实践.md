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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y2X4WAB2%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T230041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGRO0xnB8xDHRZWbi%2FZ7gn6QQvPVyTsQ6ZIdzYHB4%2FKaAiACY5VQjmoEW2G8fMopoRacAgRloqsFU%2Bi7H4FP%2B1WOPiqIBAiX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIME%2BD8OROHR%2Fh53VsZKtwD%2BVuzUckkxQQNFPRghx4JqkI3l1qnTMUqxD1upmXLypyhejQ3HaZBuYaX5HNTc0Cru0q1fyoDJxQisps69TBK9zYNenHYY1zt%2Bugoqvde78ZPyVTglTWm9fbv8yrgOhiTxC3dL8GnAj4KkrRLMyED0L14wOyFSYNroJC9qg%2FrmuIJzlTazO%2BUK%2FsaeDc6EETQA2YwG60%2FTFj2azxG%2FGFhHOkYZMEJMHsorLUAdQm7upHDn1ZfQm6WsvHsT3SaWzrGLPSjs7vTjtiKRJ3np4a40S%2Bev2W2FN6rQm2gt%2FvTG4g%2BXocXcH%2FKqyRZYZMo55UBmKs8Phyeki02XFBFRUh3PX8GJu9RZlEGk0UTuPmOPQ4MV0%2BBPDsMgrhbMinRMN2lleOz%2B2uOMD5zEFtLq4%2FWP8IT42upMPovyjiRY5HB17Xa%2BpMxjlhrITBmH8e3ysEldUQi7GnHPOLdgCAC7ynru6zd%2BLG3C4v8IcNyk5KgP6OS%2BZagvRcy1mkJCIoKFrYUdm2TW4e7Cl%2FBPBOT10kJN8XzDg%2BEEiuP6DkpTpKLL0o0vPco%2FNW%2FrGUwyb5LUt2p1hi6gkUqHMLmvsR0FHDFVHJw1CautRlV30zABWNJRexQ6i5PZpLYZPuGXZsw94yvyAY6pgGEBD1OdEG9AWzSGGQrIAjByHRMgOViBOv%2Bc%2B4HTa6PMoLcY8uiG1heN7gSvv2RkSelcLi9Q41LpOM4VbVwy6QhWA6Bzxgon4QkVyxWFiN5q%2BufqGStzyCPItsCFLfDZ8k6StjheTJiOpbQOyUB4XtDFuhVf5f8tzfo8WXDChOib13dCv1fupQXarAeDfNEhviIqkHXtKpwNbs83Dyk8RKMszXPJ47s&X-Amz-Signature=70397855d892e508fa95daf3dbe211640425da4205c2f039dca121b8567589c6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

