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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WBJDZDNW%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T180038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFnW2yg19WMiMvSrBDJ9J3dBrdruNvimTpiutklEGSXHAiA3fbACvtk50vBLNQfGMuLWUi8QoUNgDok9eO3RhK98Yir%2FAwhKEAAaDDYzNzQyMzE4MzgwNSIMvXltWVjmodnbwjppKtwDt7NEbEmQ%2B3UeUOF9eM%2BBAuaMJ9H6aZDvFhJRs9ezVLZUzPv8ja4dvcz6NDISll5WibHGfaDfe3Mle8QxH80%2BbBfbtm%2F0Mra6xtJE2k7JhhhRXAHSgHUXMu1yQ7CXAO0aN9TkHE0lZ7Apxael6UmSVa0HDwQaUW5o3HqdgpI7ZKKBxB%2FfF4oVqBznn8weax%2FKT9DeW639lo7Mey6Biy%2BXEaDsVKMkFPtI3D%2FA0URc5NuMAZeyE60xUP%2BpQRQ7XdMJds1M4AZHGbJoBqJaewzvF8UCp4d2JZDEXG4jBauKYSxYiP%2F7ZnlhhcqYrZh1SVrrbVX%2FXA7iFpiM6rhgiau8dXk6pHUuQVVQK13aIE1ucj0NMplYFmYDoKNv%2FazpZPf6r%2FZK7fcuwrZ%2Bpj74WyhSGUeVGxOQZfsvYFBvHarCdjdWGBaswpxSc9OPra6pDK6VjDrJ8vlO6MTc4BrHBDAjzPqfkgOLyjrofNGWJUQCHx6uK6M5UNqXiCjMJOR3dGtNRHEo%2FoJhndOC5dTF03Nv%2Blo14geXLAb8yP1%2Fcm%2B8QvgfG%2FeZlWOcGQlN%2F7a0LU8o72cYYcIMFfp4hqzWw7sbybhNdnzdpE2Uv4%2Bbz6lmimtBH6pmFbdHT1ZqcHIw4KGeyAY6pgHUM0ggAL46QkixoUgAIakhFJsceIkTyDvs8Y8cjD7vvJzHqDqyJDupRgI5fq7XHHgxwdF1VKmhZ8vlaK5Dopbb0r3n8YFWNbUGIF1TK9kWCvmsz6RHi8IMUoxpXZS5XV3cLsqHGImI%2FJjO%2BfdFmM6WQkJJLGjCfzy3ORdKvTbWWQYLorD9uMrMpqitfKItFcDOqPmaaG6BiY7hCYB4bmTq6JgkWVzC&X-Amz-Signature=6ad71715578b3f4fa8f7e76fac7f57a523dec40d9c2d387230194d676f9472f1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

