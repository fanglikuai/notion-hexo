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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664FVWJGWR%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T120040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED8aCXVzLXdlc3QtMiJHMEUCIQDDcOQBoFiRN5v1pMaNDLyyHuniFBO4CmEkwVSYBeApZAIgITNcKedurbhpIXddOLcJtYTscQNKrHzMt0A6WveeixAqiAQI6P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDG8DfX%2FJJuE%2F0f7ehircAz8DvwIxCFI53qlFJHbcU70m0BbcxwzR4WYS%2FCiu%2FA2j1neaEoJzKBJ9de%2Fi%2FbaxSOd4SIf51bIjYFlK1Bn9AOHAfV%2B7bekWu6rzzsDdcXdpGJ2vbEYOLTkcbYEAR4lS4zfxq6zsazM6oFa5j1eCNyDeCV6%2B0GT7A1%2B0MkO%2FoYbFTV8cRr9I%2F87UxlTJ0f%2B6uPP61Of1SvFrQGXtgvY6Qg%2FuW5r93OV0aBvOXizqCWPhDccfG%2BwFJ%2F%2FeQou6mCUcWK7TwKLINgDvyeEVn%2BFM2R9ZzOHPEQunI36tp5cV2Wd9Vl%2F1nLluxmj2HuviIuF%2BekJqCDEcCJY80lpNy%2BxVCus9XfNhF2NM0iLAWz%2BaTXeZTjY%2FTvThNXnl%2FRYTCzOoXrYv8N2Kr%2Bn9pm1idAiSJtYh4z%2FG%2BICKBZ7oyFDGHl0FRHpxF8XsvaQf2tO7AAC%2BwxjvnKxLrgAbr1Q9%2Fs2ED6Nqy6JQATKk4lQTLvB6XATGNRbpcSZEQa%2FOpk1EKK6l4AYP4bKrBsK2zfj9pHbxdfg7yq9Ks1I0kg4UhpHXs78uMuTKENcrcoAyR3ag1e7Gz%2BEywuznWsUOXqVMjUkfCZ3vIXFoSus0DMAa5O3HnuE6XX4FuewgBR9y0wh0MPm218cGOqUBrKV84JMlpqW1Nwu3gY3m5XMarOPDAUO9ril1d7aGiZwyVSzDEJ3ZPC%2BrfP4SsJPOw2%2FgyLZfeh3yE18Xl2OuQYfaogipW7lPtsO4qWqRPeAoiisyrQsJkm8nrnwmVbIFKbtbxGQzdWjmBxjkN3%2BiIvxHwGqcDDOW2nMSaNZ4VS5Xvh8VYDg%2FzURuJr9HqhvDTfpZZzpyBfYN7TPbJEQMj3uDApla&X-Amz-Signature=028c2664c6d0486ce8635d5dc6789098483f8dcdbe167b55299a876df09e9d61&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

