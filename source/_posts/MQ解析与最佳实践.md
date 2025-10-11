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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SZH2XGHA%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T090140Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGYaCXVzLXdlc3QtMiJGMEQCICu7se%2FqKEZGxrZDNnN2RFSEbVkNdlKgGxHh%2BvNcaybRAiBEorfZqLKYdLUdpzRAp7Ac%2BqXNacA9NjhQU2ZEjmngniqIBAj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMljaLBpB%2Fq88kseVSKtwDNct9FZxQZWS%2FAC%2BoWeLQTXGy9cJSmxTQE6AOm3rv4m2jc2l22EW0rDCmoRLx6%2B8xDsSqNOtMyVzaJaWrQCVAuXzr1aPvqQd1GV5wmVPHDyknNCoBhfjye%2B%2Fwf3%2FOzopw5cFmk5pdw0wnIZYo%2Fb5ZEMVGGTa4DvlejNs4RSlp9Fv5J2bxkSK2%2FCZdj2HvIjJOQFnp1zEQl2rIpk25quUbHpCjx9VYqwAJicBc1sthDMopT%2FUt%2FkCU5nsKivwIfmdszM8np6el6OmBKuZqNlPfs3P06xnHZlqSNe5QDvDf4hBdRIBZDlk3XQZqLUkBlI4ofMHkGfhszFUUY8ZgD2oLzeGUjmH%2FAsLbHKZFu6tR9H5xxO8U03G%2B6WWGC%2BoNeGNjoZFWURvwBPx82JYDMlaNPZuPGiouFKrc1BNuTYbX%2Bygy5MeFZ7BCDcnDr%2B5lUSAIWt1uk2goaEsQjrce%2F2jGi4JCFnPoxhmHDK7nRhtIJ5640kOvAtt6Q32Ip8%2FgZIk8F%2FkXv%2FTGKbYr5CZpXyoemsdidtaWvI3fykVI8mAlnT2n0orbAknQpmG28kUjeemobBtIlEPC9FFqjqQesFp%2FEwlwH9AWfKHX0Kk76pKBtqxaHWFyBCcVqfl%2B%2Bakwk%2BKnxwY6pgHOUe1g5Osm3%2FfQvWNRCnYyksUDBeZvBCzFW1V2BoVhzBUW8K%2BgX89ZDecV3RJRR6i6zhdA%2F4QLFuYCXAHTGnCoqGBUUHX8tsGsNn9TRZhwb2%2Bj6WXlkN7wtz0nxNq%2BgvyJmAeGww0qpC6HrrGonXSwxWw71qTyfW7uutcplzB7b5Dh4UVlXSP1yalWRpijZF667W8ZebAjADwUhPrJhzvVMbVYHQqE&X-Amz-Signature=870ce005da22aa715486ed78ed76f94c43970278ffec76f977d4c0c8e35f1713&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

