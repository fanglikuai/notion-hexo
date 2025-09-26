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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466356YCOEN%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T070041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCRv3p9xS9vK8rhsc%2FHF0wIypaot5b2qNJ%2FEKIgCIxL2gIhALC4ZFu3RGEuCZGEc9M%2BznxrqjIaYwySrxOx%2BHtP3e5JKogECIf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyRl1bUOjWydTJSAtMq3ANo4xlhZXO4iSlde84fHM9%2B5ZeUk1uESB3YukFcPYpJMgmLmpK2GCNTRSApK3zGp%2FD322va41MfrcrLY9vXWr9QvR3SnYQz1dZKHKe272ZAFd9F%2FDAfnBARhc3K08qQXygZ1AzlzUQ7L%2FjFMP564XILb9FsLooFpboWtbCrWcG5j4JpVweFNgi8nrvHvjMESNFk4GdHNZsnrZxNGug9fbvgrF4kX0%2BDGlsHTnc0Lecfe2aTr6lpNNJKxPcLfwb9ke5DgH8Mkiw6%2B%2B6KG92%2FGHsOwUKxvZsSGneql61j6nmxUEXzxdQO86v%2BUC3R7XlGcjlD%2BMs5h6%2BZickzD5trGyJxSHHwiDrrhrbGg%2FsnYLtpRzd5q36%2Fq1DkaNIyCvAV9MatHV6H6rbIFPpQHCvQcG7WTId5dQcJMj3kOYgYfgyuw70AtJdJax4WZbnPIVMdpWKQFBduSRVLkUzKknu3Fd5mCMAj8exd0LRZmoue%2BLCxD%2BsqyAHdnIGeqmHx1LIfTvqQnSXHADYDpBuAzORgJIZVVT6T02N7Zd8f1PDTe8cjLRb8loY1M0T04gfgrIG2vQb0%2Fdh5eeITH2uAg9IZxv4OLecVI8CjFl6FuhVnsz28%2B2JkjOoLRqt0t6KmsjDB2djGBjqkAT0zISnIJofSztL0W3qsH1PpUHMpRVqs8KC8cSTLJjxxpnN1oFUVqL2HDtWKcaZXZDOdA%2Fc8UJyPkPbvP0cviTP6yC1Stoftxeq1H%2B9JhPJdhJCB2v%2FdC5PlM8k5QpPDR0Kg%2FKHI7cV%2FD3x6HTh4lw5KP1TZorDpwgHNvyv0PQui5FH%2BT99KJwThVHgq817ZZW21gg%2BdB%2FNLb8Ho7Hl926tMjK65&X-Amz-Signature=491afb09a6802d1cc1c6894d25867228b5f247383364d12355f7209a6cc5b1c0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

