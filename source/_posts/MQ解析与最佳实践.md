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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UMSQWIDF%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T150058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJHMEUCIQDtftWTgUGJA8PY57CVOscx%2FTPeMv7u2smX74hlywN%2BrQIgPmVjyIGtTI4n%2BCP%2FQ%2BZI200zOHp7lmmLkWZl6986sksq%2FwMIFhAAGgw2Mzc0MjMxODM4MDUiDE8U%2FTX8zJFZTNjHeSrcA1SmpUzzAAz76hUTQ1wLw4YLWus1pe%2FLDHuJ70U%2BhCnFswzRsmi4lD2sj9X8PY1uob0Cp289yYY2gIp1C2PV67G%2FACQza%2FlPYY1zn9pLMcp%2FU5sy6hyMZ6F5UU4rateD%2FXKsM8VEXmfL%2B8wk4zobioSkOo6wJ%2F18TEUImvLRs5fjIv3N43iT4aOJ2jREnEVuk9qbZ7z2CWJZye37xJ64wY%2FfEZ6eu5tCv7SmiwPxdGzRPyWrcrtRzWwC7XEHNnZym4JxLQTzBB9ua4MAIJomiWv9XBWzuHwkOr0l8ECEEEjzFGH9dRAf5BcIIkC7RmUGdcaa%2FCbt91jGNrRFIngMJ5AWeFd5cobqWsA2cbxxN3Z3CLePcVRj%2BrKYiKA7veJERnbDLgLViVXDIp6xK5yG%2B2AxnUef9j4VsIdNUCFAHO%2BXldlinvVZnkzO1uXIhgXgSdB13L4l3BojHm2Lr7XzgcFy4RfAqnFrki7%2FmVXnNHnDBsaKWxQJZUCxm8TkOsuPaKSlTTZ3qQaw16YPhv1ZicdQyViRiFsRuc1xIx1gHQ2G3tgRR5TGkR0sfo1ge%2BrfQ8Cf4mduGMKy9%2BimmpzD%2BdkGWuOkqx68Z0Yp0aMMJni2PJHzr0AhwCyyQ%2F8XMNGkqccGOqUBtprjTd5zks%2FRsQhxDduAUWKSHIDg%2F%2FUm0SZH5VqcdAhABY9rGgg06H5bgx0dyHWBJbCYv4sFml5fpqmJWEUmcsCDPlTfMmyRLk%2FkoHRSl612e%2BMi2u6YEgdOkGrFhoZh5F2QGvJouUntsyzXpoqm%2FBq6%2FHjLoTLl0Ph02IjPRteprR3vonyJuZ94WebYbJwJ%2BTmdep1orQgju%2BHsmECMATjyELeF&X-Amz-Signature=38bfd9784d63b153876289412f183841391cb33fbd59b703c4ad138a5006b05d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

