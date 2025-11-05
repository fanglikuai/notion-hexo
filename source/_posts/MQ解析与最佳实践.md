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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665G7HWCV6%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T210044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHWs8f1HYi1kewMuTAcWJuyfGLJi7woLge00PqtUaRIaAiEAxVvvrRqStxc18UJmdGwGF3wyPS5vgFio2FH9AF8zNE4qiAQIlv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPSrDLotStp7SeV0eSrcA%2FuFwLzOCfU76jyEGIXoWQF5cYq%2FfhqxVfnbmFNsLnztf69Bu14n%2FKSyphQ0DzSax%2Bm4JcbpteVMZDdyDwlK3Adp%2FYZbFqa2VfNK3U49xf4ZP4h%2FX9Rj6qFE8dk4ri046Lt1ZZwdwUPxmWTvqgHyfuRTIFXotdxRdydzsJ4Q%2B3hCztlTU%2FaMinuB8c6qDgYwL879A9bOk5RA%2FEf69jZYSx%2Bygf7q0wt8VuZ19fyTI9MbaD9v%2Bg%2Fg%2B4B%2BqwOlOad7Q0Q65dMlziwAvVhTKhVdcit9IRDkwvrLvXyentwBakOGFG7McFKdt%2BLg2JME5cMw4hY7WoviuQw9oxOcheRZ7yDm6eKRHgiqpHmd9EC43UHL%2FWe%2FHIkc1GJGk7UHwXXwHNjSYArwB1WRzjhCKDXs0%2FJmEQ9BprbXNQcFcdpInPqX7qK6XBkGgNq9WdM8U0MlXwuJCVHTU3nh6VKhzEZwQ9bq9MMZlsMujnBAKps6ifNdZk6vJAtWvG%2Fi%2BpwIxZB5tIDPXBqOZZwjSelV1OkOggn3%2FeXx27B0t%2FIMDl%2FlW%2FexQzJIPTC4jxOqzTYGT7PX80bW15%2BNCJ1CBOCM1ABn%2BI82PlRyduvIQmsw%2BfW2U7owPCGLvnPmfYcoamsQMIzqrsgGOqUBzBe1kedSCI60ss1GvyCrYWDCHfWK0ODJEJROGQqLXwO8Bf0%2F%2BNOYd5MZZe3tZvT8YHgVO7kIWsVjNaf2nLlPOWjEgDiyEGMqKDUlUZVvwH7zRfO2mLl9fSOAEWg%2F71MbKMW69z3vCdvAMZils0UHDdQGaNnoOY6h1gnvqyoabhlOI76X3Xvkpn3Sh2pzRP94LYD%2FaIknzzZLgDQrKYRLnEMPf16P&X-Amz-Signature=997982fe3ee5070d20b507cdf23058415c886e00923cc3969c2fd80672cb67ff&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

