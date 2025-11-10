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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46672N2A3UK%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T140107Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED0aCXVzLXdlc3QtMiJGMEQCIH1DUFimS6mfG%2BSVy2il05tJkjjd5NV6cG7sz0nmZP0MAiAEps7vrtWpJU4oQhUDGmFx9VvINzlkimW9jQYECMbvJSr%2FAwgGEAAaDDYzNzQyMzE4MzgwNSIMhtnGCNE6Y4KCGOU2KtwDl7Wsnyx8i25AEgaPiulChX4NyBFFO8ITBP0mWV2BeRmzC%2BmvNMudsQYXnVANJ3nIxGj%2Fg%2FcRVYS7XeiPabjYs3NOo%2BYcuKdkmva9ftsc3pYAtuGCoymytaf2tSPZblVUIftsMVd9934HazBsYty69q2%2BhwwlRV62dl11ONqm4IDRn9XFWUj9ZS5ygfOWEGmZjDMQoPQZUbRON9bw%2BDBBwJw1NQSteJc49%2F9wq5fEROwlc5mzG5W4gDlD45Y8gBEjlalsR66RYQFjOxrVHGmWbw7gRu22egM4wANCHqzY4aMKTZTxWv%2BcAqG32XQKoF6c5NquKnOUQt3Gltv2ZYA%2BIO0z2MVnzARZijUb%2FNOd24G%2FgXVAADVJlEyurJIgNXNKHHsf0hSeJPu6B4mkFttMLB%2FC0h9PsgFPWDn98EGEW0bAbLXQuq%2BWL%2BUO9Z75CRa696gtZgas6ozv1jXVi1HAS6bEFTEY1MqsEtqq%2BKBFtey1FuGBG1hPY1t4Y800HpVJqwb8Vo5493ZmrZEsdoBhcv%2F0XK2f0zBy0BDx6GuBPFFYoBVGndZdueyXmxYwrfc0SgrWar%2BIQz1NxYxmhsLNkKQOSg%2Bon6mXhyXsaxXSeIbXOIERFdoK%2FoSl1ZIwibfHyAY6pgGT3%2FJNx1t369VUFHCJ64LmuaBtXZUQCF4nsLW26wlj3l3o0LO46ILQWoxAf0WAEcS3wD6%2BUWs729RBSce%2FAc0sQJaM%2F0tNJ4%2BZz7wgZfsJElXCHKgQtpAAuV0wyPEiTrjtb968OZcdmlabjumjgOHaJragb2%2FGMKQYR3zlT4p6l5OoaH7MLxXebziPx33v7UmAIk8UdKwBfFMiGiKL1LWBKC3Tlceb&X-Amz-Signature=59daedc790480ddede7ba6ddc2ca556f913f0b5eb761fe55a7db30e3ff6bd53d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

