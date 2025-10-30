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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RVUUNRA3%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T100055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJIMEYCIQDNAZxABxkW2snBI8y6IglOLNqHxq6jWweU7JWGsxCnCwIhAOpKwOivFEIfT48j7KlZtRZeYuVYn00oHeb4ECeetevnKogECOr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw0UvF3UdZVJ2nXq2Iq3AN8kOSWzCb0hc7wFonIQ6PTiecOcwjDs2kiRXJhi7dw8AREWAKspTTtkoiY0aVwzpC1cH3bPBh0GX98A49LAsBY%2BI0oHPthz5s2nJYZXtdy1yzZ8PVY4A0uTbMV87kXMNWo%2B%2F87hdaizAK9Ug9d5Q9XzA%2B4kAiBv5SXo7nL%2BU8FmKS9rqZCiKH6G1wRF8ukW8cfxiXO1Wo1CO%2FglZ2WJFQ8UKiLLxKgCZQbRE94D%2Bg6hc1MgLONybeZnouXSvwydjdVQTDsTLHbQnkmhUtvb%2FOs0RkYkpGK0XXlXwAzWfOKnlbIChUVI1FjkcpZR%2BVm2QUCHBSQkP9AvmDM20Ost7OhAfPQEqG1EfUbKBLcp0K8sIHkfwpnPoQlGN77XT%2FWYkQzNTgu%2BssP2ocaraeXFhzqyZMI0UFvFDyQBCK7gBEZ76Ps4GHKIMo7YcgFbOC2jwG9GDSecLjKy%2Fwyz23kP0BEKOSNkmzFZMQ%2BdQm4ArqdDC0YInWyde1ZGObshgWJhqq8dN2ddOqyu1dpWOZ%2B4ClPTYi5jFeIF30BxJxf0WFQSl3%2FPToO9sfQmsbvNZwmWq3DlXcF8YJWO7PssoBwv4TkFlmV5W8D3B%2BZCYeamVacwqW%2BP6CEbCjfvSVxSjDy1YzIBjqkAZ9DdZuP2dkDHssuKzIs7G8TIpQ4AQ2cXstDln18OhCNcI3EE6r1FcTjXi3nmfnIwFfLIQIvFm6V7bpRYy1BswrbUB23l2YIrMvp8w21w%2BZ4k%2BnrBwrxVxOXcmLS7lsueHxX4h9GIqQETncsipCovIEmbEUSws6qYSCYSToKy78wuNIQtJ8mGvvuYC41UWUnb0Q9Y7LcA%2Fdcs109UKLQYN6zW1On&X-Amz-Signature=10d47aa57508f8e4597f87a6c13dc27ef945844aa9516da097588f91e5296b26&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

