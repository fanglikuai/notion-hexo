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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WG3XXE2S%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T140050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBGgLHWzYAICqDFX%2FewYprQg%2FjamiyVPiF82YlLlHGP2AiBEeS0YPfjFb0wwEo1Igm%2BAOTEjxcv0QlQMKXc6EgYxpSqIBAjF%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM0h%2F3pSw8JwiBzdvLKtwDkFgXXm%2Bf%2FchblOn0TD%2FckxGdFkfz9ieNhu13LSs5JRI5iHsAzQNnUPfrs683ZzoDXe9q0eePwq6syTO8Ur%2BiZbRjTAvW%2FCM8CSD6uGk4ux06cBhow%2BzlRSaH6atiJgRG%2F59ZhlyBg3D%2B5uEqGIxxuxV63d6ZFJsaZ9CWaGmVnbs4NIJf%2B4PVH0pGi3uhTIV%2FSH2ZXqdlJtuNkmyK5tHLLw7jlHlsJFHAXpCgv1SNrBlXiPmRcVLhNekx6aSILeePYEGLlpMKk4ivPrR19OOndi%2FOLKBjdT1UqxBrhomqidfoiwYZm4ELTBnYZthv4FeqUVH3BfSUa2hOymEHRNSWTIQRyrgvjRJfR4qU8YJBCfqWzSsGDD5J3p88AOxz42PXSULq5tEVQPp4BS4CpiRGeavs0yg6WPQq9jt%2Fdh7jwVgQNohDJcpvYKk4DWA%2BTUBFxuihb7PuPUyCpixqSJMURAw2t2ppkoSZS7G0Uxlv2BFipB5UNcUWR9u5FxZrY9KLZ95tto0iUGCq53XEZUjdGUO9IxxZt9Jf1fSbflBcFFNesyHHkBzQlh1GU73Si0sDQStMYXGCdl5VL42OGvj7mFE21DdcdBKX5KO%2FbPjgKwsG9CrnMT9MwJzLokcw%2BsTxyAY6pgFyibE3vxdRjaUa6P%2B1%2FDajMnRBKAQbyuGKd8EVdhEAkpOHVeqh5bB3YCPPeH1fsttGpfhbflAKRgADqWGkyWS%2FLyQ1wx%2FeqXLeTVK%2FwsXlVCeUaOwqJMBxUEwTCamOt2oNBZg8b7PrzQWaX%2FY13%2BJ7rjGW6JFZqk4BT%2B7v8mV0PLjBokTR1MI1jpI4eZCUUc3suHijiHiv6uABwqc1LEoT6v4zjBTf&X-Amz-Signature=fd4c1c97e8ff8ccaec81b2274349ca05bc3ef9a2d4722b492463f20682626e40&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

