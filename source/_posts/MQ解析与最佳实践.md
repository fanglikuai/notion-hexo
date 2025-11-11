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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XY2U72HV%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T020041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEoaCXVzLXdlc3QtMiJGMEQCIBvMUX99oFGNGO1kh63oOjebS5acpnYOmCmB9zQK3Tp2AiB9WuSWCwjzqGhhNGugEOFj7S3tQj5HzUevgNjYn3JcLyr%2FAwgTEAAaDDYzNzQyMzE4MzgwNSIMjCaNayoq8zFsO7XmKtwD79PhFTZSvvtO%2B%2FuaAj7LhMhdcznJ2BsKR%2B%2Fmf5C7rlwM2KNqb7W3hbhC95wip2A8HmNcWlI6g%2BdZAiM9NHNjwl9yTxpXdFn9sDZaNY0g5PfkO2d5nVyzuMuDvDMHxEFOjXRl1zFDo7oox1c%2FSsSIORv49jBQeeRuJbH1T%2Bej%2BaCiieLLY89k4%2BMoK96KabNcuTpq%2B7I94jSQkocOa3Z3nvyNf1UspkhA44%2FAcUgld3govqKP730JVASfaT9ojewn3m35SyMKHU5Q17WtWXlYUkW%2FSWMHg8mBHcRVSVECNew1crfbSOu8WU7%2BFO2NdBijKP4SRXcCtJY5pROwX7kvmBQx8iR0KuDfzVaRkuAR0k6rBhH8xYD%2FOthqkp8O3ObvSuPVyJIXwmDb3rTqymLgSc3INzL%2FJuBfetFL%2FovDQ70yN641QW2Deh8AVvxdTgMdaAXlppsgF2%2Fx8AGgZDf59bnqSKENWtzMsuHgHdfkOtHuxDRV%2Fka1358T0UQJDCfXm7X0eEmDWvaCPpwOtRfm6rvhz13u9nMkdzclhMkfa0AQpPovYMUHEPrwf21Y8iNidX9jDjIf4NrlbH9YuGXH5waaiW2zlT3pcKw%2F9UEOP%2B51%2BLizT0ic6wErjpkwzKnKyAY6pgGDF9HFtfxjstH7tZnLYqkWF7fe9up9jgKxNu0Ov1WCxNct53Jirj03DUreryjr1KtIRGzdc2iVY5j6nPwHvSMhnPzWqXSQLeTrF8fEr64kYxuBFEprenH75Beu52H%2BXzlg2lC1JvYD7sjNNwVd%2Bie0G25AOv5xHNEswm8YU9YE8S0QSS2ZuqBj6kKFmqD%2FGCCcB74T2I5R%2FeWhQs7uqnUm3h1d5KDa&X-Amz-Signature=ecf8d96f1f45f5fdc63653289433d6ce8530b8e5ab93b99b8ba81f2783c4d48b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

