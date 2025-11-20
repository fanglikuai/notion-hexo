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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664WQVVCHI%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T070039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaCXVzLXdlc3QtMiJHMEUCIDUul6by0v3AQKSzJfzU83HVWx%2FiYPfN4MXorDtTTIAAAiEA8dOXgCFyjpn0Sjxr3Pdd6A%2FHsp4c1yIVE9y9kDMlhZAqiAQI8P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJ3%2BnzUUsdPeiKtttCrcA7fufBYN9NR6nuuqwIeWkP%2FfJp%2FRlWulqcpq8F5fnmycvJ%2BnHd3UbHW5%2F%2F8OjrazAkBeSzzuc4s%2By0fijhqOrPqBVoNOcmW%2BxhVOBwKiUSZC8J1xhhuqBYwxfA4g%2BPAnnyi2h5tz35hBzZNhYlcgaDQpjGxGOpDOdnMFJOGaiL78265uCfmwSSZdIzEnoAWVFP6ZNRiQCtfMJQjWPl1g1DOxyeDH4XGVBjXiB8Z05XDkLHclJcFia4liydAc8W18T1nxZcrDw%2B3O5lHoPI38YirvJqpxLZijI4gadEthGe2qBcfKi%2BwoEHgxV9xtF%2F3qe3OpWPgCfTb2Hx0FPR08ijPRV%2BQj8whdvi7mJGjoU2nBI8bkNhcdMYHmpz1nnuFG2hChoOcHU208%2BI1j0eiL%2FtEM4IeOVyu%2F5qg%2FlWAQ3zNGuFDB3RdwzKXr2t9T8gfggXR%2FJq%2FxKepBlRr0WXxxolkGCl4exSQjeK9o%2BaCa%2BrGB6G5%2FbKury5dTeLIfsSM3d5dCSyvd8eM9foh4q1NRJp%2BxgDVlJ5Cddsp2SUxIyP8biGtGG0oS1%2BTsA1G%2BgQQq2yUP6Ea%2B0OmJVRs11%2FIRStcNzMnh0Vx%2FF3vTqaNX1xybZFM3P1nrCrYDVIXYMJ32%2BsgGOqUBuzp%2BtnVLWX1Vy4skyljww2L7KXWivYyrRGl%2F1xY8sWm%2FYaBRtBozxTEWlCsvSB6dmv%2Bd87ws3TuZhERnSDZetp2xHBP70VvA13zK0z5Oj2e9ORilKOccnDzt9XpRNzD8N5mqN1dX%2FYLQGv7JDuJz%2FkbNwQaHYmGRprqicjpoxmGxCzuzMQ1E4jmiRLhKeTSarm8TZRLDmU3ANQp1MaxVD%2FYGFevs&X-Amz-Signature=e4d2f92b9b702e3ad9c5380a1315e514787b333bbd3efa336575f9dfc3293a9e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

