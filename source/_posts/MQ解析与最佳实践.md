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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WZXRLQ5P%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T210044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH0aCXVzLXdlc3QtMiJFMEMCIFHbcMa%2BU1%2FDtW%2F7g3K3Cxhp4slCcm4yk0hYa%2F6FPrL5Ah98yqnj8QgGLeQ4WOAsvhfBKm6xVZLOEZhMBJAX8AhSKv8DCEYQABoMNjM3NDIzMTgzODA1IgzF17JzSU1neMTfad4q3APYwqhL6zYzgLqNHKJcH34kPzQI72ZGu4K1RQz8x%2FpZX%2FJc7ou1PVkoueoRtGn9FBidC2fy2D6zVECgJ2IX3qriKTX6g5dJL%2FBnf6g7tvrMQuEiIGD%2F0m%2FwG4UHYqhkMkA%2FpWhzV5XUgl74ygx4oUfKOC%2FsVmAirOupYt81L%2FRAvQxCh0r9ohtUuEH7MzNBJgD%2BvyILuiwY4hATV4OoJUCVua1yG9P2jeWxt4iJwMnBzOCEPn3XaTCRTeuZz0ZdSo%2F6838JfVknp%2BunP68aLvZm2T0HgvKJJI8HQiMVgnV6ipypMhYfBDRTS64peS9oNksmuSY0KjDoEKJaHFGF9wHjGWfb%2FVEaIULHV%2BgeQQApHKzrIwshqu5Nr7fcc1cbNk9Nqa5E5UgKOXah%2F8p7rDssGQ1sMhGLEjkoyUk%2FDZvV5aODTIHGhfoRkYt9cYfWoiiC5zY%2BEWGkgv5SotDDMmLgPyXL2OqRBwUl4Nhr10aFS4oY7vInbIMfVV5j8IgtAGYiSpeztO7hearzp3CPPosbMp0T2cvRn2o%2FTf4CiB4tXODQdvo8co222TUsyjqp9ENVtiAeFbgTQdoB6H9SkvtL6W6377SgS2%2FdHUNJXIEdLj4xkpudbZH7b2AmXDCK2o3JBjqnAfgIOPiWLFXhJXeNU3VUOpKP64FHIJ3efdjqR3Fia3Peg2cyv8P7vKI3anHEoYTicg3zI5YJwi%2FlHvtz2pJcfIlm2tQgzaEwk903mXNJ0j9bdgAVYW7CfFsSf4u4SRbda33oQ1E6YiJOVjCn1bqRLh4zoTqrY8ZJC6M3FmJRtgw%2FPlsUn523x3gn1srpruOKtpt8Iq0Yck5ZfoeUe7yQ1AVp93hIa1eI&X-Amz-Signature=9bac8573fbb24d43d344ad09308bde99bf10edd53e943826c6f5356ac1943ba5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

