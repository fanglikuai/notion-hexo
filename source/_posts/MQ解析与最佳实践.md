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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VBF5HX4E%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T190049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGMaCXVzLXdlc3QtMiJIMEYCIQCs3H0m7QevWf%2FbckKCImS90Xo3AoBjWTuyftAlODYm0AIhAJ7Kd2x39ZH6b4IPIlxsl8Wpp71L4jQckrl5tnf3VQJuKogECNv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzaUgilnfruUMjgnu4q3APK9FIAnO2C6th79vy4%2FHNwLu9UORNJ%2Bje1gyVdnCXTu3lig8xRqNLTrYi4PM0qBOE7kx%2FDcQQ%2FgbsrRJLaAKUfQaRH45Eam0WXvo6bMuF5pK3qBU%2BNeK6IHtqZx%2B8j%2B%2F09Xy8MApj4seVNRN5ovZMPrS9pLHJqeUMRpCf22u0s9wrEVq2f1nvdkuVCv5EpenF8u1LrE285Yv%2BcBSaO39%2B%2Ffmr2tJBD0P4nD7KZppe1pFE2neZJCIgvtxkqLCMrLBpWbsft5TpgqHJ9zv3xOfxAzTv7SiVf9femFcRYEkI0CBeBBybOc1P4fKYq%2FQF5wO3rUOD4wDeLSErnJV1ZwWYJ5P6ikgUnJxnM55RqAHBP5baOlNeHJP7sLiI21ievQb2SDQiOulrIpuAhy0gfRQc1Hed%2BJ7pa9MkgZX5sPYUk324Qel6vvrakAbuOfUvhGr9l69HZyj3SXj5T6%2BkCoMrvOf%2BrXtt5wBROnhCq%2FsS4ancO9DUw2bhqTZ8NE8oZoBmFbHah74xdzCnzlPbWajBbuVzlW%2BzRjfh5LdNTPmsxYOYdnIm4f5rNhhFwaICNEyNBmh7jXzW%2BXIJAnBZVaEmE3PZLyxz8qCkDLqT1fgmGI4I%2Fng9uV0xRs%2B0fvjClwLbGBjqkARa3wEbh4yg1xjGY2tzbnKAMdwooPuKeSh5nKofBO1Pn7vGpjfTTYBgINyclFOIUOLWUiaT0vqahJZ%2BjklAk1tb7FfecVsR2sjmZxXsBBZAYGCzdbZ3Pbvy3wsm%2B6t0Ha6WOBd1EejwjdDQb%2F6Z0KcbZTA8yP3rkUYLBV%2FoLE7TGzY2K8cncVwLa099b7UZ0sQLHbTON4f50bso5M6i462mbZ3u%2B&X-Amz-Signature=24f8cd4e7e76fccaceba94768a974f05fe41845ba3df1060fc8d872af66046f4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

