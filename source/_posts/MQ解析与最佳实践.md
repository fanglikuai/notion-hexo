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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S5BUPNMY%2F20250916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250916T155517Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBcaCXVzLXdlc3QtMiJGMEQCIBSNMuLAV1WXWaSXE9vSdWHJQEvIVJsOdHpW6xgN5ISJAiAfMCOulbIRa26FB%2FTjGGrGfgGEFZpM%2Fzz7uEUIhmYnASqIBAiQ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMUP3kmBQGida1GHtjKtwDTtu3MAZzV1kZkqKickDfSl29RCFopbkh73FWGZPQExln7Go933fq8Bi9gGtcaX0zaN505nZ8MQFe7wsnbdCUGLeFgWGJymUPhBr7qByHX1238PCz5tmM3PHbXmGOasUikxVo%2BlHdoYgJlFJptyr8gGGItjg97Fz6mpoeBQl8lQmJ29Ta0Ob%2FUAwL%2B%2Fvbiw6j6JbTcD0xhYoDZjiX3SFeaJR1op8WoiOUKmL3IMKIA%2BC8xx4Fg%2BJIm83Oj%2BOENolnNRlfi1%2Bt7BUiiiwhSu4U%2FUqstPHBxzoV8jzaSGMRU22cuZPEdPUMnKwCzGUiE7GuFwoXqlDw9pf8Q1cw90LFl6RI12aecUDU3LQgPVRA60TMihYu2YLJZnAK1iNd4dunRmuRIP37hTbgaWGI2Dy0KJYMRIVGb%2BUfIVA787N6kbN1tGVoqlaWcamoo4aQcyFG6IaCJYAZuXmzXWH8OT1fkcgWbC99iQDNztrL6uE97QNks8qp14DOlE%2FWsy3z0o3%2BOiaypNYvArp6mVwxbala6IuwJ8G4g8eaHpQtHmSpl8ZqVz4r%2FW40d6wGFg04HcLyqvgoSaxcyL2e97Ws4Vuuh%2Bq3kqx4KmAfPoxDFPdf1DdJ72DXYCGbrqoP%2FFEwsuilxgY6pgEn2d7%2Fusx17eJRT7soIC8REVKjRVdnIB2xfieSFxxQR4TdtBJvtMaQWFFqrclsE0Hy8LRxaV40GFd8Fs3Iztw2Et9X6%2Fe%2FIZT3a3b2O%2BFlF1GsJ8DnugTkS1l8SBInd63qD5whBt9cla%2BhCuspv1UZ2Eus2oqtyY6JPmGQnUaZ%2FzF2t6ZlnoS1bJ1MEh6MqlgPDdAhLdtmO7KVoC2XDfwdmFp9YEBT&X-Amz-Signature=c6b5f712b3bf557604b601d4136431b9f554b96d7325abd45f7f64cbdb4cf6e8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

