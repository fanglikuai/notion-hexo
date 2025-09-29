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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662VNVB2LF%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T010039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEEaCXVzLXdlc3QtMiJGMEQCIAxN8QUbT4tUSZLlEj0%2F8SvFbOgoTwmz6TwIB%2BlWAaeQAiBvEjCwYzeJdYn7oUWgWv%2FXoqf7hrjXZ3LuoI1VS2HN9SqIBAjK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMSyfkMOupWglOdat6KtwDg0lfrztDEYzd3QWDnNwHJIkwiyANWXoG6phmIGJf0beehrL8ZpQ8rkO08%2FND5fw%2Fl%2FqOPkYssG18cIP2nW6KzT8tXJ5nwVYxPMBpj%2B0lPKqvTH%2BS%2FPcNIJOjCDvPAl00tVvWGoqwvW4iBtA19XbAqlj%2B9oYRlXZReSB%2B3I97hdgL1KcdcDuSBCLV3H2C2COcv5uYA8egqZcfAcTIXQgi0%2BENMLimNmT9IEIRHZK6tfbjItSV7J15DA5OnYKPmVetTfjanQpqff6jAZ0UACfLIgL2YfUBQa0Ir9EnyuNUmlX3eJ6TpVv0fNZbS%2FoDioK1uA%2BL3uePb1rXeDlUcK3FYlWts0vPVkbxM0GYR%2B7wypCojkvCFrUw4Q9YJKkPsBWFkkB3Xs7T1JqQ6LFVHzClBcHuLfxrGMRBETOnASWje8VakSHZSz07gW0B0OyCklYXUTyGeQ7FoxF4v9Vlkb8FwHicqsGaA75u1ZToeKduhjzYM9QV7P6MwYLnpWpc%2BvSOZLqHmwW65vjRvViPnKcYNSBeQbt0vh17bQcDDlnfQMaN6B7Jr229j7tI6JN2vgfrHDo9kqF4oYlJQS7lmBYBoqk0NzUFdGlPqNKuDvYTzg7b%2ByrhlAZ8tXk44pUwgKvnxgY6pgGYcCgwt0yQPB5uR6anlg8HNjbMTrg5qcaZZwjbC%2FmJspJuGcZ%2FSllA8WOU9xAX5xPabmmjm87nTppDpTvbkac%2B3r0QoQIT%2BzUh3zEv77K8aVPXSlHYuhdFHtSgyFZvVhrSOkiPDEjVEygg3lIGHl%2BBtOLWmGKLDQ%2F6SNK8PFINyrMErCbeiG1xhSbrgQT%2F4UG%2FQIBOXCZ3LJ8w%2FM2i3t0UD%2BS3afvt&X-Amz-Signature=b7c50643f875b7667a3d9440fa3b05934d8b824cf71464165811eb3af9de9c8d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

