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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RDTEZU7V%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T220039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJHMEUCIEFuJoTX%2FypmiXP1pwaqZTQ1HyYa40jHuz84GN4nnsY9AiEAqn87kikh6Mfm%2B4c7lxHQHe9UN2%2BDZ%2BP6t3tho7NwoDUqiAQIqv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCaI%2BvJY%2FJxQA2fcZSrcA9iguulaXp6eoAr2MLAkGT7ykCveosTUpmd8qsUGVOXoNX9BdGM%2FfJ%2BN5yF%2BYLFNx1gBFTbuHMkmnTUUXCuHh%2Fe5LJGCVUe86GXXSa0YrVhQQkkerWXjmZ4pTPD0XlAIcoTKjqM2YXB2Z4NXfGX7UTMBjEurF%2FRr3oMjrHxSMxYOZLWqyHmy2m73T8n7U%2Fs%2BerAO6oT4cs5j0oXIt5Se%2B41StLOQ4wqdj5EsZDrohGL4hsohVXu%2FiaOD0brL2%2FcLNpz4ksrOB5YLwCS5nJCJ3A4uxg9w5jBR6zHFLiEtuOOK4yZOF8vwyYfikD4JSdJAu5ne8L%2FdIt20KYPMkChTV6V3jirntADCTz5bauupxy9X5h9hzQulNADK7KFKjP3LX4xsUy8OJgP8dgw%2FMTI1%2FhHxQw%2BgqKOwciQOghbA%2FKyE8Z7S%2BmeBFGnZf8g69%2F5SzWVR%2Fug577pUPUxkwvAI%2Fd7LUgSVgu9LEKE2JLJqrqygt%2Bx1GLFmVwHIYuoeGeT4b0bVNh%2BXJzfu0FflRuhQYrXrY%2B6shCk8aDAAKbjJp4FLGy1rpJkFAXitVeRtqWsqXM2LADa1rq3VLm%2F4kyMn%2FIn4X5lOSmhy2Qsh1HfNDSrn2tFDui0RYQFCEtBhMPrUq8YGOqUBwDCA3q4yhj3CxIkXm%2BbgwROpBCliiKOMRKzB8bnCwJpVGuAMCXndn%2Fq3DyktA91zY2HBCXw5SlDcKGb88CXaKF1CpSCjKG9ATj9dR1CzQ%2F2chAmDtBUfAZtceHcFE%2BEMPGMX0VB0SWxNOfhgLXgGg8yB67B1PpQcSncnnlwznLPeO6TgqcmX3%2Bk42pYgesQW%2FgqVFiB14LqU0HubpyJXLo4UYNPP&X-Amz-Signature=9517f820fad271d57d1a0f591164da3a9e5fa18791c373263a48eed9af43b3a3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

