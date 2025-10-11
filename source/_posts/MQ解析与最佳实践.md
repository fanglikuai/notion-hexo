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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U5VNICCG%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T080046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGYaCXVzLXdlc3QtMiJIMEYCIQDm5AoEbVmSY2p%2BLBCyuMRkagf1HjQn0A%2BRhiGnYvGyFwIhANVbtb2pOXmyjOePlPFU%2Fkp0o4urALOj3lAycaHH5riIKogECP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwXR1yJXG8jf%2B3cjHwq3APmDdK06aYExddu1A05MGYFWWoCYsYGCXhE8UYrbkUsWmy8zYFLEOU%2FpFzrKTAB2vA%2FLlZFc4veUBSWNbro72vAmUGn%2FNQTCFBDfadyxUMGmPytcbrBXiqUshVYzAm%2FRHE%2F9b%2F4KcMq2fzhxa1U8huCNZ9mIHgrsDSuD3KteAzwu5yr%2FQJZt%2FJtqfKmjocbJG1mqwP9VXL6YY5NpDFb7cfx9xWEp9Ft2ytEX40F0f5EGKa%2BoohysYOkvdC2NBrqSFwlT8%2Bo6cXVtLyup700MYBH5JabOjr62%2Bn8R1TbbvujE1DJjIkA4GiOWhXkyk85dzP6EOE4ArOXy8k0TGvUTfmvaw1W4Jt3dAQMRkyMR27j9e5Pl6HjwPRPc69hhBmry4KouCheaU35Xm7xBXUpSoLLj%2F%2BOGVAqbQTGeEsjcjGFq50M7a6lZ5MshP%2FiErtoZgLzf4FUOmWTs36mob6gVoVoX1l05GM0%2F2FEe4HsUKeZliapFgq7Yv4iPV8rbz5LNHIdWh7Y9Ix7DLK7X6PQzWKki%2FOpb0ZoMyQ744rHFXitzF0n4wb%2FQf7J%2FG5lA996lZBHlfMI1r%2BQxXGm0hBwhK3tsIwY2RrXvXC97jj%2FXup3fD42OHyeGKnUqZOk8zD74qfHBjqkAfjTYRcl6T8eWlu9RdcqFjmoWa2yNrrrrTn1s9l3pafj0kZOYuCjaqJV9stwgx%2B%2FtFEUMO7KgkEvQuJN0RrA0aKOEoDi7i%2BjPbH8HHYt7mRND4q9BJ0TeLu0rRqCh38SgodaBp5%2BWp88SOWWFgB2%2BOpkCdCAiBH%2FI9mwaEYwVgEAFN46u%2BkCyHOiJAev6xHUuS2Ktcpn6nsLfT4eG89afQesjf%2BO&X-Amz-Signature=e068d90c6de94df73112e4a7af7922304a4d1b2323aa24ca2d3c40d908d285cc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

