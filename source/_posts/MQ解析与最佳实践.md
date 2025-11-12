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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TOHQYGWU%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T090046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGkaCXVzLXdlc3QtMiJIMEYCIQCZN%2Btku3QtqSUJVty9PsgYj%2B5zbIQbBySX005LmIBkQgIhAOm78K7HPxI2OQXjqpEfH0tFjzDIHUgM3Ts43kWSPlchKv8DCDIQABoMNjM3NDIzMTgzODA1Igwn1CxNRbg33YKvRpYq3AMM%2FWCrbVlL53uGpQ3xG%2BdpNIBJDJ745vIHfiaxyxuCc3khg7EgGbgoYgVcaI%2FzR%2B2wuPUQrknzHSBQCdh8cyFW4oeLXxtFquAoFdZz9NLOjdahHyZ4JNIfqDtpmlX3e7CgtZCEh1Rh0jOK2B5kKOGtl3KxWjQJWnni4dLhsdr8T9Cjy5B6NEWj3rO3fCquNap4%2B%2BKV1AKL50uIX2IN2Lx9MXUNVz9VQ4S5s2GxyUiQ1XF4LCGerqvKVXHfgnztXDjoQ3%2Fpw2LLX9BjXjzCtizRjG3gqdLFfZOc19%2F8HaWTtkg3DmZP5%2BCnMaS9D9kRl9kpjH6OuM5Z335G%2Bt5bGS5XjZe8nD3rxrENr1sMMepa0DZE9ZmloRURG%2BHf%2BfnNRwLrZZxIsUa1%2FoxcVDtFu6rRRUTWTsaZwT4tIHED4sWabnU9aCs1J72HTgcF%2FME81UBy00fii0xUxc%2FElXu1VbeXV2KdstN4aqCrLfs5kachxwYKodBgZGXK%2F6qrMPxw3MGDKbUwDL2jEZj4CDsaS5CrvJnCxUsKgOzZb%2BkPwbUKfeLzk0cs9wXzsK0B3dNxQy7EaY8Nt%2FWg0kpy61LJafnRzms1dPTN4nVkJNcoJt1MdkU3IXF1t%2BgvKycKpDCrkNHIBjqkAZTeK38YvbwWIPKWWjc9VPMVNTWP38j6PAc9Uo23ETVWwPElA9zPXcktjvROmA1zd7VG%2FVKBGxfqcV4kRngI3s55KTk9wnxwnUjqOW1NxlI%2F6Zad6FACj3HPQnF%2B7Sesj8eoqIryMiTLZWt%2Ftess2zozPZ9mTnyop21jZkuS3Z%2Bb0EsU1n8H4UwESPSf%2B6Q4IWtjbRRdJ%2F2XePkg37JbQjp%2FSWMd&X-Amz-Signature=5c15f27a5007c9e0c23013ba40ebd6b561f7171543d9a11301decd2096e9a73e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

