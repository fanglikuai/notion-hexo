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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663Q6CLRFI%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T000048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECYaCXVzLXdlc3QtMiJIMEYCIQCtYaJgiBCt8FWRBADUtXflWjduV8loQjKX56%2BLbothIQIhAOP8OGwknFwAikz8IN54EFdFjNF8Mb505e8BtJrFmUvTKogECK7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzXo7TuTYtBySvBgf4q3AOD2YBS2drZe5TN%2BphrwlNGMA9F4dcv01zJbB%2FVItxguW9FIvKa5%2FwAYZegjjVNn5N9791W3z60ry0l5olYei2dWyo0U089ScCRHPulE0sxgjC6P1CPj5PkZ5yHo9y%2BRmGAb835SeTox4PvRjpQcIEAPwnaTHOPOFm%2B%2B2UFQCZr5WjNHGEji6P%2F8N%2B%2Fc4SZQktFUOVJsf6GUGmZ133A8WzWpK17lCI1yKFKUGuK35VCp1NPmsTpbFNQZRURchQeWueoXPall1dPgkk%2FIn%2BiDVOQs6y158Fq4DibDtPhPUfLdp76rVL6KE1D8zovfK5L5EaQUD6ZMKMiS8%2BjFW7utkAGkHnG%2FtVv1dm2j82vCg5GuOw9KmYH0bB3NFn6WvX9zZBmklTYkyMiXTwg04AAW76tgpCu0jDKYlcZ1VzXqcBZnJQyjiE8d9f6al8pdCu2mMlC88tx9YsyNFL%2F2LBeOyuCqa%2FOnvN7QC1%2B8zgtWQElr7SXGwaOY5VpP%2FQchw%2BiN0dISwdZP%2FFI2pOhqe6Z1%2F7NNro912BAuzBnfIrhl4okdsp03VC14PuzHlU6TpH%2FBMmJQHufZE95o1qmFSksfmxROEK7s6JNxYtUywS8Oav4jR5p9sPfZ9jj3QUC7jCRquHGBjqkAR6ca2%2BSeY%2BKXIlUMFmpFD6HSbuU%2FXip64iMyA%2BZDQoE9KVeIrCB73GDBJvvzoZ5YUyIWVsGUvAlfGc28Lld4UdZe%2F4Bk0NU0ef3oC9Ad8KOwez9NCEOCjBW3A%2F0n67ffa6yNuG%2BnOilQGAo7AeEyot8QgsfnQWdYvTEGTSy%2FW8nXCU4I%2FUXtGMDX%2Fgei96tPO42dXzbnI1Ugj3pTwz%2BNEAeJyeB&X-Amz-Signature=839c5b9442befabc3fb867bccbccc64f3604667ce54c387188d327193d2ea397&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

