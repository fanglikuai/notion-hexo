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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666RCMD7MC%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T060039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDDloZFf5EINnO4u2xE96X4oCwmFnctbm9DsaFxvzzLvAiEAzobdK3vQAdEP3tT8A1c4pnw%2FqpOCu3Q%2FNCrlrjm8JiQqiAQIh%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGXnFeK3FGxgXHkclCrcA5zHV6sOAu3Kr6l2%2Bgm%2FxzubasItifrx%2Bwb5dgxMliFPV5lgADlMPSAas28z2ws%2FjtqGDdGZcbDA5%2B%2BgyQx9pNLKiBa%2FnPCmA0mT3uqk2bARhyZkiLnRdP4tktJj4NwwDhVixM6f2%2F8CTu3CyVxMF9Bq%2F7c6L875VkyeSc8G8FZ3hz6arYsnNv2VA9xhw9KhYUbqOF8nbubXjmY7mx9qEs40XH5rOI3SUR%2FW1%2BJH3loxkiNWsT1%2BOTTQWlVIPm7HgWoDPnT%2Fwd3yj5s6kFv0q0REryWb3RfAwpbqhAcwOkerp6sN9PeFPZsWxfSfWE8hZnnTGKIl3RWFiOKPVWVcQSc2dVyovp21Tq2YoCjsSQFOYbrCt2%2BeKWtltPSSAwPywpCAT4y6M3oWk8Smf4VEq2eUVTZVDg5pfJumQQASnk6A0O6DBcDo1FGrH%2Bm2jeeGgLv8kuLlCEd8ypt39SUvHt9WWtIvyd8qPigdyW0Q82giD5wybmWBg6rFGgRBHjzY2MhNeIqF5MQ3BlsAQqqpHEKz7bDzUOAnosjR2PtdihIOG%2Bqux9hSWghYZaZiPrl4XJk%2ByJ4T4OGINGjPH0HtoTxD2rBT%2BrZNZmpMegRInTYHY8oz2pZyJzB9GIbaMMXAq8gGOqUByyVx3xiK7BNLffDM741mkoquPhYKCp0kwBJzqFpYSywLQR5DPWzUmqKFJfXXgLOdtnokXT60viLGa199Q%2FoO%2BI1FCp6Hg4cODlFMBRDkWemct2JbAFF16Meaum8eXbwPhsftViJUAU8QIu%2BKJY8vBV2aSpLPaes0WRfzND7ASq60w4dL2LmO3U%2BNALBRDHvp59SM1h4nvkotgnDQGX0wWvHKto5w&X-Amz-Signature=9bede5590aeefdc49f15c512830bb3620837cf1fede4ab9ee712133d8039952b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

