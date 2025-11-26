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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664XNIOGIF%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T210039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC7sCXU%2F7MzDiR0bMrWs5rJCr57gTFCfDudCVB1Q5zYJQIhANpE0%2BJp0H%2BWcMWeWuU8eybSJszdRgERHSYmOndf4QfYKogECI7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzmnKCnRWTJ%2BD%2BRJBkq3ANvaCSh90VqN8GfXNmpMQvsv7dHqESJBy5iGILMsx6SYsvqRXCVQwhFRDVoVHI6jMiVlVPM%2BMR7jRTPGD9STGTF9fAjA5I3sCE6WPxpr8YvN%2FKpHzKrT5Jt6H8vj023i5qHoS%2BePsHt9YDFxQhkG2EYLXvEiRHLxvcxDYirFzA%2F7xuZOKMRAv6zpesuUOB5njwNY0a1E1OwqkVu%2F0y7tS%2FOg4cyDPZ1vMpwrghRTD9%2Bsg9Q0HfgfdDtkfD7o7dIiclspr%2BIjuXUPvjLN266uTa9E8bq4I5QqZ0OcUMOkwoV%2FfozC%2FN7IDv57cuhd%2BcBSEwb7G47Oer%2BqD3JgZGH9Hr7M1kPbuIwThPCf3uM6NcbaGB5%2BcEuLN3XE5IQvBl%2FuKX%2FKJDjIP%2Bw6981RXzPITPMDABkOoPfj0JbLvPQGrDNaxbH3U%2BzF1y1DFnvW9ag32suhnzPbnGpbKtPc1qcnWikyOgIKQJOsm1p5iXcE7OinKlyJLe2QiDjQr%2FCscChrng5%2BETfJwrms39vuaY%2B7pIOQNeYAC%2Fm45oswWT0RFQilo9i%2FqiopK%2Bv%2FblZVUX0zGZMEC%2BjJDkTqS5VNtzFZbEA9uIPCx71NODq8%2B2GClHxY09Sgvr02MZQmBfNDjCjxZ3JBjqkAdh0YC%2F3QGeAszjO5sSA%2FYynxlMVQW7PmZNYODs%2B2strsW52l%2FC1b3I5lGL7SEeoCDfRlSlQoHpEOvw3KzEdx2M1fK9aaRqAtBnWDUEr2gecsp9cBUoRuGUwqKKKGv4aD9fEgOed0k2JkCGVlFMm%2B2AHu4StgadOuD%2BmoJXV70bSk6X97SfSmjUMk8MUhVnDsW9pfvCW929C5A49PBAhY7qrojfU&X-Amz-Signature=dd62647afbe6e1e0b123e93370e5cb14530483f9e4d179bdf23ef246c6bbb330&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

