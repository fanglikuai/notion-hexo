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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667P7VTRME%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T210045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDs%2BTVOZ0Gl6HG5IZYZptjBYkcs1a20q7E4xA3iBViS6QIgLfpOlM3pWFVXMXVHFuCBH85KWyhRBp9a3QMQ58QlKfAq%2FwMIfhAAGgw2Mzc0MjMxODM4MDUiDEBP4OCnt6qYYY0MgyrcAznzrthM3UHfk3khL6qrdJODhvMvXDGMgkw1BAATWDm2huxauDo6%2BnlpZXiAq174MDeNap6s2Pmf6Eg1ziMwH5FIUH1sdH3hjkGzUXbgnhPxFWMIvB50uSI7Gtu1stLJShalDyKfgllxi3shOIp7KTLTwbX42TaJNGy6gKjJzkeNwK80k29JmdVfzj%2F1d5echw1gdtxNum7FsvQlT3PVHvtPKHfHvIpZbFGkIiA2ogskRjUENrhz8FXApshtOG%2BeOps8szwRaMFTh%2FEN5lcGCwEniQXasiEri1%2FsTw09tT8ihf8qe%2FDyxt8MzFy376LN8cMKPay68YI9TAzZxR%2FVGkNcCTQfK3zCJhbAgTmUunvSeDhlYkLOsZ1l39Hd63Bv%2BLPaS%2Barl08HODBdQAgqyVj0M0wBLswlCDNhiQ2dC%2FQ6H7DqY5yfn1A3p8fvy5jOjxFV0lY2R2WBacvtmdz6%2FrTDY08IP%2FOpeDcWLwcPB%2FgkTv3XBev7qr7KCJaCM5Wz4x41cD4C5qDdjetrIYiOIXE7WcDAK19m87fU5a%2Bj1sg8xLRW5mqP5TCpowbx5%2Bbs86295Ue1s8ynhqR%2FVGWhRgX4hXvZQ2812cBT2odXWfs2RvwSxQTERpqgY48uMJjY1sYGOqUBcCNJF6VSHix3SXFAHZnPaWd6Z8xvA%2B%2BmD37SStXea3SkUdem9QPvqsTDqSh%2Bz9nKWUmbMRMT%2FWVEp3qOafHUOfJ2znYagnf4jxmX0eYw27AKh8eINg12GZxaMSzLIa4g1tr2gRxirQCRo7YqEUMNGn2uUS1ZXOeyBRzgwmgswh5HTvAAJH0OeW%2FjaSXpIk4AHjWAXm%2FCNtcMjdg3H3in6KdEBqzK&X-Amz-Signature=cca87892913ee9c621f2b150b95700c668b23bfdc32d0ff299a90e4ecacde3cc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

