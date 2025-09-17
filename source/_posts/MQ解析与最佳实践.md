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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SH7BQ564%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T070049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECYaCXVzLXdlc3QtMiJHMEUCIFbk2J5%2B0fvu5VaxgMZW1giGPNARWdKvLUT4AxYJx4qOAiEAn9I5oCPWTcbmXZNuuQgrlUmb8gwHQ4%2B2kxef6cgE9rkqiAQIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOuF%2Bj1EvTA%2BcLcYKSrcA31jr9pS5uDqlnwN0uX2ciocbbjB3sDNRTydw0LYCDMGXy88tn7P8rksekFgGwH3CWH2TktFzFubdQOGnQeVhPL5LFq%2FUbeqXhRCgtrtSVX43Hvd8VAF%2BwAOWXw2QHZXJ9PaKxCcjI8F9HGk28RlBhbV7Pf78s61QENzYslMIBBtSYq0RCpiXPRlTadDInWpcKklZKwxOs74AnDry1aWrL0msMvhT44mh98JBSwef%2FmGQXltAOpsNQTiQ6pD8laZKoUxaPHgEeB01NXxzWLHs0sVZM5ljOsriV1BZD3whWB7hNm1rLHn5f8PmAIEGbbl6YkdYCH9cjJp34zWPwZZX8nDVpeEJ4vc%2FoAod3iSyRFI1asmDQeTIeHLqSAcR2lAeSK1OOEWP7Bk2%2B2dS0eSlK1FSGL7%2FXKGZ9ZCFoBm9BB79WVss5uiMyqage2ECPXZX7Bfv5BQAn4Sn27Px0K9LE1t23i4wA7DSTPdfORjHgFEWl0%2FnAQNmnKd9BRl5a8rp8yqiFV3JFRxjZyLuJxpnRI5nQQG8UrDaFzeObFeBAya3fiy3b5DXsCkogMv%2BVCm1i8Hii0iKLTW5G8bSxX1y7Me9zytHz86G%2Bb2y0cPuv4ucgj%2BlCHCPbw8V35QMLikqcYGOqUB7Tc9yp4LfCR%2FOHxM4o2i5cQ5AdJ6QEOEHe86ylCHD9dGjpB0hCoihSPcZe55F0OptK5m1CzeodHSoq%2FBdUHSp%2F7JBpt9GfuhvTHNRLtigY0WM5zdV9HSsN4OC%2FxtZg%2Fya%2BBVlUr1LMcqvgZBGmS3sZz1nJOyjXEp4lYbHnpVumOFX8fSLNouAppIMKzxkoI%2Bwg%2FxFjzHHa408F8RgjJNqvq%2FKt7u&X-Amz-Signature=f4c40c99409749d0d526ece90fdb266fb3446c25de4cf77012f4794ee0a87742&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

