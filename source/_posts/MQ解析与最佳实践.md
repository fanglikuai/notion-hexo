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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TYUGVQNN%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T200041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECMaCXVzLXdlc3QtMiJIMEYCIQDDjFEY4kgPfKYqSKDUGI5Y3RH%2BcxLL4Iut3rTAjOOFvgIhAIvPro0DdpvfdvK16OUZz6igDQq%2FIHSZD4sId5l0YOVZKogECKz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwRE0iyccaLgimw%2Fgcq3ANNd04pcxLtp%2FLW%2FvkVjJ6ETF3QZOYIpr08gC6SYA2%2B4bj0a7MyuPWCME8vIniLwidbCZZYpWpPi6DRewouV5SAHjN46e6qa45moVTdmVSJQLOHYw6P8HoY4tO2vMoqGDsb8lM2RT%2FEVMAKQylJa5jU7zHhJqk%2B4OboUosIvCCkIEB1ft%2FbFYodSHNTu0adKWf%2Fh0lDkoCSSyD9OkxpQMeQ7vGbQPxrRL31cvMhJTWvceUIct9jifN%2FjEvgXnJ4L7fUWZpFKo2JUzej1cvvofen7nIXliqVV2UwXsvr9i7W%2BXiRSKcWZ29allRe6ENWHbuIlgkHJsnhPUjPSA95dngbR7W%2FRsLj%2BGcKKpjcr26fdYzsuJ3hpS2UTjL8FiP6rMMIqaaCz6COVL0UmI%2B6MacZ%2BAFvVlK6H8eJVCLOD5hzj2mbiYr0yIk18Ey1B3mdB5LcXkCkajN72NfLGIvhgEHROGoBLhZlUE9oyltAtiKeO5IAiTz0j%2Bvh%2BNfMOCjEfE%2BzloTExdw%2B7Oilxqf6V6PIMrdknzzW9OBsrgPicIwv81fI1x2oQJQAFeqpNV%2BjGIGbsFcO3zWC2MOA0IhFgZdXUfY%2FzMBFUCMDGB01mml0umohUUEbScaT00mG5TCg5%2BDGBjqkAYhWjcpM3RrvdFgV1%2B%2BmzCI7dU3KWayZEJv%2F7q6uJ9CCyc%2FW6G03UgpEsyZsWTGByAz%2B9OMV%2B0jaRD6C5ZCCktGrNJnp8U2cYEEkBESz6%2BLbxhdl5XLHE7%2FLY3%2F1rrrQvP3KWTnclXf0sW3kGSccXbeOTjiJxWQiVoClcw1kRQl3oJxHzRLe6EKstfKf3nwDaP3NBv125HjzL4CVYXij9g2tGkOc&X-Amz-Signature=c23ff458d89efe1b29ef48ea5741350b25fbe24e7214670affe70e69c7559130&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

