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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QYOLKFHL%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T200131Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD864pbWMPPHWYsJng%2BdXl4lLysMYfciPnlJzPG2KEBmQIhAJvPj%2FlCszgF8aXU%2FXM%2BmBUC8aGUxkRzt9%2Bao1sIXhagKv8DCG0QABoMNjM3NDIzMTgzODA1IgzY7ziP1ojNZQI%2FGaEq3AP5G2KCrzFs%2BP3HJvJY4sAgbQo7r0j6j8auqQ5FnzZOwQ0JBTT2%2Ft88BcdDBCb%2BjS2rhP6VVzDeiMNGoPHFs5q1gTZvyWJz5ZYnk9GiJ1HEEqH%2BYXrx45u%2BKiKGKL6ksgntcdx%2FjHP0usohM8QTa9S9ApvCX860IoYQA8i0IVLacZo7%2F53mJCM7eynsbvo86YqxW3SxebvCvlYyBIJJ1ye0PZtdpuK%2Fn%2BUPmGXSSg%2BG8Vpx%2Bqw%2Bq5t0EDXaMKAMYql29Lb9RuRptYQE%2F%2BLagr8QkcbrmZtFzEMjLezopSfQXxuWUU4pIW%2FWV86QzhWMucSRtR1%2FEf0TN7ZY54JmvHrcTO4ftU%2Ft3sZZt9znAz1yXptJrEsm5olSBtETNbBu20dD3T9l%2FW0OiEjXiLw%2FjF4XiAS99wLFmNXipHIpjb9Q8iljI0pPzhLMpVt1FE0lj4AmpllH8IEBa9CsnTPAuT9vdsHz9hwcypKXTKEOfzQtP1pR2nQzzTQujObXM2eBegKUTCNPwne2SbU0Y0zdtY%2FlGR%2FbKx6Zb1Pt9kUkH5qWQM8KQ9fQO40Zw6MzN0AOWMN5nxa6tS5c4hrPyVhj15tA2KfRy6a4YQm5INFBwYfjN5DIWK6Iql23vSXJejDOkd7IBjqkAevzTED%2B%2BRKDgv7Bkrl%2FZgfCOSqoEufUGqPDhy198U23OkQbwmyxJOysKFR6I36bw9pqyGFUX%2B9TM35Ml%2F04oueT6hsYyxg7i8pM0WYoNWYsIjqAHTGKx1WtY1sdHQMMdPH1Iiq9sh1xGXYVBeynMa9OW%2BG6rpm2FjJq8vjKMHkiTRdZquwW4xrjLwHJULwQmqHD3RaDDmtnYqKdlr%2BJ9Ob0IjxK&X-Amz-Signature=d437bdaac14aa760464666f3b82fb67d09ba78a2719c153e71a610601e317374&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

