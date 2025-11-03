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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UXTDXBXC%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T060040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDJwT2%2F0l4LN%2FCVRALZG%2FKQGULC1Ui7rZBL9BUgqWORVAIgDa%2FalpNVngXH416Cz3vT%2BVSzsytF%2F08QUEhLYFRCN8sq%2FwMIVhAAGgw2Mzc0MjMxODM4MDUiDF1XdEBUAeI0KezWSyrcA6vOVGkM0OqTOLZj3hRHQnd8yh0z9fd1gXzyqmmCvMQRda0WAJRoXY3uQVYQQDWKPHlRuBOWghjqfBOTrNIU%2F3Q6jtXgCfQSjqonbkLwDAUz2Jnypz0aQ76S3QXnTYk3k1zKF9iuELPxIOQp%2BlSig8aUXyJNd058IElCBoJVjV8TOpGa2RxU9B0ppuTTKdDBMM2J11%2FuqUgNzFgEWicjfq%2Fe7ua5tRIrPng8KLw43r9ckcdWvcsFfFt6jQNUaQIQh6l5%2Bu1Tuxp1YT1PfCrzWV3yZIKIQ%2FgObubJ%2FhJ%2Bt1EqVRoShHKXzHZIagXnbJbSerbyDNQ9mn8gCnpQXtBnLrJHltyV%2B92A0guN9FBBxEyTT3Cw2JkKfg0sKVXi761X753NtxtgHBU%2F1E5DnHKKF3LFWSMNE3JUj61I79GPIemPxMVAU%2FVtizOvNkkToIXRmXuU9p8l3MKgEspwqAC%2B2BNVg3BIKy7gEl%2FsY8eZw9RAUmSS%2B4XULLZCCpKtOMzvlgleOxxstEJsPdNQMqHcPQp3RdTDPWE6D2GvqjkY41rhWMrn9C0nI%2F728SCnW1u8lK4MnrNwnIJZfHWUuDm7b7vkW5ZrztK9yBCcviyDomtm1seR5dAs7Qwa9PcSMMjnoMgGOqUBw%2BrFeeaS0GqF50VhMUZKIW4CDj8irwedOZzos%2FesEZvjytSTMS6BWW2%2BgkH7uil48Bz9HHN%2BfHG6tU9sllZyce%2FBknMXOMNSeS7KWZ34ubjYlBhndwWn%2FierHsOTDjERT%2FfVp1YSqt%2FV8L%2BeXPYdQZoJIjZrIW8rkMLubqshG5AVqom5JAFvSWmV4xCfGikTiWEJqfgCLQsYLLp8Si4PZ1yiH9FJ&X-Amz-Signature=d3ab0abee464a806b431fed7a57e2e48bbc2d514368ccced023bb11174935692&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

