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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QAE4FH2A%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T020042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFOe4RIEDA1DAOI5RdFiHjoOTdOY2iyZEqYhGMg4jZghAiBQrCgfd%2BOGLT5PZCCl0ZhOfG67vCJOuMQE2epnHT8R5SqIBAiD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMJ1H7n%2F65QDNeAA06KtwDf5f96yCu6LnRAqjCq2SfORvD2huqSRxX4OQkzzv0iYs28%2BPxP5UhLIFyfA%2FOW5Y9NQXfEa%2B%2B00I2xu4fBVHz4AHT%2BWSwH9Lu3mUTuMAyic4DiGVEyEDC59Bc9p6g2wFSi7Xmz1SsRqrT8jqJ95wCVF7KH%2BBivSm8PuIwb%2BbdzpoZrD4TK%2F7InGA3WsX8ZimEaJP24hIGVjTgofaHVa89ltYwVepi52kDahX5Iz%2FawXshovXWvaai8yCWNbeXwFGuYXZOvB%2FaQefY6akH14eTGElc%2F3kS4Ci9dTdUdrLh1A89WPhxECR6iqNYm%2FaNYbubLSI9IR05vCfqSkSYvK37OsL1%2BCypYIT%2Bc%2Bqece1ns3Md9P4D066ggeevu8pEEbaFXxfn%2BsWd7aG5xsD6FMZZ4G9MzK4lcCrcPdGlrAtYCpPL1pRbXc3GOC%2FB86ogvUay%2BYP6fnrDBubRnNCwgv1HdEuksBQGyrgxyXNKFTawlmDImTm250WwtRuS3gnGIuynvREt%2BKgypStNu%2FhHOM4JLAN08sL4IgXBkDwWpVf8%2BbKVkGbtyrXkMeYn9d5eklhBTUeBE4ZJz1agUsORNX%2BHKGPnq57ipoyLiPsbp1anyRj28OHoTi4jhCyUdE0wnZ7BxwY6pgFvlya5N3xoKjiKRBMignRze8NX1RJZwbSUH%2BpZA0Cm4VWP2DXgvZbLgewkRn2Y78SKUAZEsrmuiWqmNnztpZQlt%2BVi2BErPefLwhz9%2Fve69E1XBNIwaWAhR9MKkwkSO3sS45O4Tty8g8BTrVbqNjxaXuI2mGK25pGe1n6qFO%2ByJYUuFXsJHsn0aHraGCHpgTZUlMEqq4QkhS96%2FvsNXH1PSd%2FOhge9&X-Amz-Signature=abf890226f4ff67c5b09e8cfe3d54f951bd4b12a6969b79776ae110a5f128dad&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

