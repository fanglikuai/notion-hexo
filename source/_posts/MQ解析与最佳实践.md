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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZFO2B5FJ%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T000054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF8aCXVzLXdlc3QtMiJIMEYCIQCNCH4KBfgYDoHuoVnL164xxk6Fn9fJMSFA3rPd7wqIEQIhAMRQwBJUXw4367Crd8vzn8pcScOMXsr4BiFvqBainj2SKogECPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwsOGqC6nOHBcvgZlwq3AN3gJ0it9FcEOOz6Y4LBCdqWcScD5J%2Fnhoxpbu8Iz3BrIO8NLEmXatpOOmPgbXscXKK3dQvR1W%2FN%2FAZvBziXgnBjwBppb2%2Bvk9MTPyYkfVjqlm78JkkVjyb338wML7xkamcKQ9Q0nkgI%2ByMkW1I5HG%2BqJL%2Bk3TXCqu7bOaATZ%2FiRCgUYwNI4o7F339mRGGLrlJAogifGq8L38zcLabgx5qKWnouyMdbb%2Fn0d97JtvK0yevu74JTYqe1Ejsw5YII1Q%2BWwp97kfNkLm%2Ft82eA2C3KUirV186QgREb7bZbPtx7LFwmBaJCmpharPDbfQBKYMDjwziyVWr09nZBy0%2FaE%2FTIddbFZ%2BXtko4O7MqP51CRMbhUUCkImPToTYOzuc05h3zXc%2B5KwSwdehEoZUnNOEtP9jJ0fL5N%2F3S6%2FtWHJe1lFpdEwQmH7cyXeGX8xU4eHft9lTePhpbiFXysfgT7SFVAaVQ89qNd1hJCduM%2FW3cH4yF9BQTZF7C6C%2BxyNyRcOe7emBF5qgArTrwvxqgd9q8sJ5TXy7%2BAi6Ww2nibs2UNa5eomKHLNEYyxsEBhtG%2Bqyn3hHVSbsafOVjLs9q9WW2SISnzT3rs9figQVsn7TxYXph1CLb1FU%2BvCXA%2BMDC9n6bHBjqkAdfE6N%2BK2zT75ebaEaQJrx7rC%2FWSspFnx15cA0SJQlfJ0QtRfC3EYb7nHh3MPnhfZ%2BKK2qw5SjyvPi2ptlrL8xGQeE8tYFDbb56zwEhTyfuVgIKcAcT%2B2n%2B%2BWhehs8y3yP2Hn4eVuFb8aNxSRoCKjxrHfDiwqksgMebgh5rP2s0hqv4QbE9gvOs4KA4rUH9ctJ32ilhgN4222hcCGXXF8%2FSHcWP%2B&X-Amz-Signature=4a104bd5d8028521adf886587cd050e42fa913e6e5b83984e69ac9a66c10dd42&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

