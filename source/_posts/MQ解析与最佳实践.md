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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YWLNDOQW%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T160053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDhr%2FihyR9V%2BAWI%2FrsPBPL1Nro0HRQZKx8dpVQK%2B5AqfAiEAmZGa0ZgQu04AvhQVC7vIN4mVR8Lpyf%2BDPHK25YjQw%2FYqiAQIif%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDN7lTFOV%2FqsfBPyySSrcA6WTHhEtv2GtBLT74AEJbK%2BopF65TwJac53pn5tP%2F1YV2OCJ0BbEaiq%2BIdfq3SDeugHywZ9HMWshQw8OCo1JNsUFIM1C6o%2BL8RdGwl7wwU8HHUsrCrL7B1VJfbKzH9pPhdtKDR5ED3DJgaSRWKHv5WfOBCRbMnhVdmQd901hWo668yBu3QRE1zJmdiPMt3CcZuxy5deaPRnAGAZzyX9fMWokzz%2B%2FigJkETHJUjmHAfHsX0qsQlgCoMMQU1980yWqv4UKRGJyCeFWHdzzwgocztN6O1fAy91%2BHnK7JL%2BtsCxgUmC2yF7KSdZrssxD%2BJcpc6RbUfjCdF8npebCfGGXmmbypeJ%2FYNbkqbfbMWchEFRSu4yXaZwSQi23NVhmBoh9dFNX8yyV2LtEhHnuyCgSei2oCWL%2F%2BsKtoD0CkgP2iTh5Sn3nlMcsPREU1G9o5jxi4%2FxChv6KnSEF8onOiOYQJrJyNk1gk4tTfuEoGu8268cHrrGmw0zMM7T%2BwtIktQd10sueXkqe3rD7aBSgr6qmYiqUmN9YQgSY4a6FZ505jzm50bBYdw%2FtEk9UPXlq2%2BY7XIrS3Gb1w90s6Sl4GOy7P%2BwxRhcRoVXAHOhE3qrfHZ8IZBLblwiWq4ld6ktoML7HnMkGOqUBNSidkjR3YV8weS3%2BtY4Dyk86lFVtlZZFDdEtXhWlzclOg3IWVWuHhclOdRVjJu214SCkavC9rrw5u1zW9zptyXoJdMmPL3jGRfo2aXvBhvUqFjhnMj7YaPGK7eECSb6JNc2RhjianrZuWGyRFBOtD4%2FGWhqAzwS%2FblM%2FePl5Br7v2jA7nqTwOcIOH4X%2BLRWx4HNnCdPC54mZKgctXM4Ke8ze7qa%2F&X-Amz-Signature=e7afad22db791ad8aab67dbde054eed79e59eb6c05fb0dfde0a2a0348ae1dbad&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

