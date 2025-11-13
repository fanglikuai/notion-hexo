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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UALPTGLH%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T170123Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCWZVekGJlG2g8PYKsUnMaO7waf0gtQPAM0dOLyfdjFLAIgJgPziuyqv%2Fkyeq8Mxz0cDwM3BV1yKj%2ByvxVylhM59f0q%2FwMIUhAAGgw2Mzc0MjMxODM4MDUiDFq19cXEd96XKMzPvyrcA%2F2%2FSY0Dc%2BzFjnWQsij%2FSvb5vW3oTn9NZuvTGYnh5r3bIYh7VO9qJ6rlvuq1hv5byjN%2FOqef9f3%2FL1ZKELZOD6HsEx03Iu0mlowF5glCt7IbasAyTLHJmcq5q5XK8mFH7GOiFuDKQIVlM2FWa4TfwLBq6BwhgcOjyf0MUxtqiUs4XiE4lMSbUK%2BjPgt4mJPfb3eUN%2BmpeNfY2qROJRln5QcJTHzRrOQC%2BbTvhfI8TBeWWPza%2FtTpSpmbY2ACRzWZOnSr%2B%2F%2Bdh%2FYqBWmzrLYyc1%2BXg6zwq64zGRCSj%2B%2BQghCbCAEPoSLsSkfXqQ6cFp5bDW5lcwDsQ%2BheYN1dm19WTXmxzuQcgdKSqK%2F1Si43A%2BbaGuFyWZ1g0m26F2QbPBSDYSC0drtNLM2QaUmWq1tjcIRraF6TJkxYSD46BASoCwQeGxc7yU6eru2zfxNV5yigBj4N7JbRNfhbJKHiS39XOxkSt6lal5P64N%2BkT7z64uHmc4gxts4exm5TSjVEt972ZM%2FsH0VpYhEAGzN2EZtJvWRdkNCKp9XbZOKkyvjgGLKW9NzCPUhKlgFQpap4Z1JJkW0QkabhINHFFQqYZeIvAwxGhz5UF59gtjgJDoTo0mbJFpLZoOu9xPP8fOV0MNWV2MgGOqUBTtsfN%2BQDy9d3gvCNPfANaubd1FsKWeHAorN%2B9rPRL2FILD0y6LdrcksN4HsDCZuCrsG7%2FO1%2FXsBdyQhklBnjUkqZkMCpxlOuxnA2NiDjPrEkRLytvGsZ1VJitDLG3xfdWFGBzRny6fUh7XIpHVihrQM%2BpBSAWE0mKrK02ONg0DEOaAMoqT%2FMtC%2B%2F8uo4L2fqkFCOMzgiSwRM3i4EDFtbYrjFwWxr&X-Amz-Signature=e0d65e7bfdbce00692ffefe2ad081bcab51c48e6013569f3d75cc6c528ba6467&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

