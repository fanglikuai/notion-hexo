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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QHSINTRM%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T210040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHUaCXVzLXdlc3QtMiJGMEQCIGkWs1tADg7E%2BHJym6UzbvQd%2Bf1q%2B9OmEpem6NEYjNp9AiBkrtp430P99Q6NOVSOl7ATBn%2Fl03wgth8WMDNcBn1UZSr%2FAwg%2BEAAaDDYzNzQyMzE4MzgwNSIMApegjDmo6KuuH%2FcMKtwDc%2B4o7rfLe7Q1VJBzMc65X6gnxPYYCuKeG9u7MGleA09DaCPYTvZMNZMMckqz%2FMAi1O9Wb0qiOtsCUdf7%2BhVWhrN7AYZA1T%2BGJdlF6PUy7HxqCUkvIcGhiVa2pLsBrbifdo9BKL9rP074kiJqFuDDRjqX%2FHPBicysatSE6MAZ6i177gLkkFDxyDxKz79FmDE%2Fxp36EvehuC9RmkQc9gPftXas2YSZ35P0fKn5lCmurWA%2FaL5E%2F7TvKKDl9bzOq8FW15aUkq%2FgEyfULduwB68wjy9HmS%2BMKaNH0AnBWhyODP4SiT1QMYrawkZ%2BeWYKYctKklvVoQo4gTIUdprAVZMJvvj%2F2quroVpfE0JNfOo0D%2FKphhhoZjxwWh%2FyrJeFJL3L5XroJBgj6mVfZRYRliV3%2FV2MR3WfLAk0wlyGOwCJRsf1k%2FOt4fN1ruzgPxtVzSuUYJn2guhwMYxMxHnuTuSh%2FrrAPektnh7s1Q9klK7EHLAGlRQvRWHjvJ40GPLYBxBqDyNTOEL65ccItVuPDq8PBlSUCB3M2dEthd7Yyz6TQzYPZj2%2FZfkKW856%2BgBmEGpbQYyz1EqLv1lfzGFvLatkAUA2eqZ7Y0yzPFgEmUbIR8aLwI7D29mNgBCrd1owu%2BDTyAY6pgEl3BQ6dIm78TCr1yiLNq%2BLrCVps2LKjgmly80RBBzTeC%2FyCzMTe8A2GG9432%2F1rzrG9wv3M2UwOf8okNgiZJl1EEv9iwc1oPYOq2HBabmE9CPtrxnTwYXows2CaMlJBjaiwQQFREjSiaJlgD5u3V1T2mlECI1UU4OOweZQSgCCw5rs3vBL36sMyFeECUxR1dIrpOse8%2FbOQztcHUhzR3G5vqAVu1b9&X-Amz-Signature=c5bfda9b3087e7722b8cf3731ef2d18f7e0106767628a15b31ad3c9ea9a1cf88&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

