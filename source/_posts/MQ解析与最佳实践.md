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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662HCSQQ2A%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T230038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAYaCXVzLXdlc3QtMiJIMEYCIQDHrag5Ka4Hm2R%2Fuv4lLgXTHFFYCibuK3b9jT%2BgXQx19gIhALAHJJVz6mZvWyo%2Fr6JT8JwfqDctql7kjV2EHJQ9oeLoKogECK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igww17CSBOb%2FkmAZxn0q3ANPhnfWCNPbXQQrHMTyVO54mzMtZxZu%2FXqhiySq9ZhiNR%2FGCdY5qNLAQ725SbUa8JO7INYPcdja1l7wRquM185yGuOZQDWNoPd3pTUF4a5%2B%2Bi3M8SzzVPbeXX%2BNueoW%2Bwb3twNbkdTa6wCjCmMvXmLPIz2yMhV6qm1Jd%2B8tiJPy1RijqSpaGJDBipHvaLPdeLmsVwOxM0LmDpVZCr1NY09sXHk3GckzEiwavXZNTL%2BilpGawRsD41HQw58pKOxu0H8HE%2BRoFkUTfP727xjcPeQN2EGyENkVCcLn3w2ETUeIJPrnma3%2B%2BYdou6glPrkLECzjQAzNizatOihzTWYScZAbgYr3IxhhuQ0nXglZu%2F6wCS55VBx2K9GUvNNFtUz2rcZwvRNwTSFVCySN7qQ97vNUOXJJOOadaa8Ok1xga%2BLksrURXie5dFH2CJYKoYV40iD7aeih316JIf%2FtCBUISAmyl7TFgcOVTw8oQ4d64PUuEpl71NZP7EogfQAQRLCEpzsaFySP8iUm0qRmOAaEOfPI6T6jCaYJsZE0G4dOsT%2BW44db7KMZFYObSDm8j9Jzi5%2B0jZ%2BfI4keRw%2F7YNqpN864Uw%2BCt0ZGJXdzr5wTYM28ye5WtvVMA5nl7Tf8aTCg%2BMrHBjqkAZ16JDKTmDUOstxI47cRoOm16AsztnoR8%2Fjn3EF09l%2F1PyowAYLhbsb2Zuz%2BSYjDNpo0QoCYsFDW4MiNscp9Sy2Lxpi%2F0WhDXIhf5AMhtUpBSbo0%2B9VTAv18F33mdaqMh%2B1cGu7Ljq5YDJEraaZQmhrhYiaRmw0%2FekDwJzkK0YYwuABeaXtczRbNAwTubGRvcgp7UH7byNF4E%2F50ezFQgKQAHuYL&X-Amz-Signature=0417ac7380e18a3e57fe85fdb1825a02bc46714256dd2197325748324e1bd493&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

