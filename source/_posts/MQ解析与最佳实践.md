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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XMENVNYQ%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T000040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJHMEUCIQDjP1K6Pk7rVnbynBJE5dpEgfHiKxkXA4DswTtE2kZ2iwIgNyYF3lejb8hCGyy%2BNBG69qwRUvftM3q0V5bp4HFaA84q%2FwMIHxAAGgw2Mzc0MjMxODM4MDUiDDHDfZgGrhmy5I0I%2BCrcAwKGFuwjJxSkg2HHwEC4D9gGq8a%2Fo5b%2FvwWrlI5ta%2BOqnkL%2F238aSmbvq%2FgTG%2B5NwGOizG0w%2FxQ0Vhh1E%2BF7%2BqGuHxtBUE8aP2UbmzslnnjnILBdw5m%2FD8gL0OWy2GEJWmZDPNeSj94hQUbOUCvttAPEDkCHqxFdEe9whXaRvaH%2FIksYWCiI1LFZoHgrqJRsAZ4BhSg%2B%2Fik7hNGd3ozaHbpFvvz7IJzgNbUpoLo0LOFKHjbrh05YXm4xRhdt0AKVH%2BNy4VE0wTZwk6gyBH3vx4Gw30FNNV0Aj8N2wYzMa3xEydqoR7%2FW5Pa4EhLywiE1Rk6iYJCv10ZfuXPOpJhWIkFXB37jF9RITIBHX7f7rBcUB8V5A6DtVDYK9ehOKecaQuLsF1x6TuiDQG0ZfIGmfOeMjHYBpBnTaAGPOd80Ov%2B4gvsP0AjQcMaFavxtSVRCO011m5%2BiGKtdI72KKDMtBP7ii8VxkQlIZPRXZEfFv%2B9lZLKkkKQHg7GRFsqom0fIEaDng4vc%2BrMyvRr1eQax47tWA8ld8FAEq4XHSIr7d9QnAxapNuhfPD%2F4pkAF7%2BxPNIF5GBZys1SEnrNqW1tZTavED67e43agtymb4pbIBxijfMT%2BOR0dItWOJMc2MOemq8cGOqUBZ%2B0O1Glw75lCgCp%2B1dt1fRs5LzipMyUAG0kDvKXdHhCH6GAF3%2FucDmZj%2B0nPITfWebIwWTY7pbCC9BJz7HDq1I2Yjdb8l4GCInuhql%2B1VVYz8ldTh9vRD4lDv5wMvcD4oRMvQjAd%2B%2FSvomeTCTOJPeHkz%2FLMWy09t1YTQbmFmkGN%2FtOR5dlPQydUcMEDKNJHG%2BvrVC9U30B3ADdWLF6kWfPgvbeZ&X-Amz-Signature=75a1f3cb644766454ceb300099d332cb39d2f29faffd680c07dd4ca9accabe19&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

