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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RRZT2VFH%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T160051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAF0dHXcfCi%2B80EaFvE7jAYnTpOznSd1xgraglD5D%2FdNAiEAgETjmqAn6NIWKrrdWYt7v6Wj3kaaSqpzO0H6iYSTQJAqiAQIkP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNpwGojI%2FKmAsBONAircA1KQqxqRHXu%2BSnD4S%2FHThKhxL187JmXIAIKp9vAN4PfvuBB0BrIlmi8Ul%2FGgBvoD5nJTL9c0G3%2F1bCXQOl2R5780sowSYHsYIbQjRY6X6JNl57B8TQHPr2tgMkkbWRMDbTS3CI9F7fJAcR5zsgy1OxNvxRDK6QQKDLFhNvJajSyYlxxgZNwj9bmyIwQV3ILIpR%2FV3YE%2F8nH2N650iFHa3LrexdaYNUbSBHUoDfaQ7cdEeQHP5t5jnO95EcoYeYIyNMIVngrXNk0YmdTBt6XBHUFpCSZOJ8EKP8wrihVlaJCKRJ2tF6kVF2ODGMJQ6Pmsuevb7YZHli%2BtiDCGUTiqFdsEIvmu%2BJqc7i%2BZqNGlbdmvHR6zmFlVA2W73hraUFe%2FO%2BcYj8z4UeztYTL%2BknWxdUx6vNnBYUlcOH1iNcPkeZHI%2FPzYi7iDmUh1zG%2F4S4QbAp0Ty50Bf01VqKBfvh7j1R%2FsXugDzD6qvCQRCHbD%2FFLEHY4%2FPoslAFbFHezHbKjtXHdV7UPOlPMIvAopPt99D80CkjGahxvoQ92KtGZ4nv%2BA6vASlC7h9JR6UIwbYshOvLwnZjG6z8AhmancFhbpfl3KkbTVRM0FD7nIqBdQdAB%2BKSnBf5yId5J%2FLDgoMPWLxMcGOqUBDLa%2FBrydsP0MexdTa0xoBfQUOSoy7rL37Sf6bPIlmE4sGVgN8oFRca%2FJBoMSMyuROpz%2Fv0y2UxYARWq2DNaUcTAWne%2BBJl30vJ6ASKJXU7zGlVU6PyebWUX5YRU4CvBzOKAUqwrD%2BTtS0CGcwNk%2Fwrstm1AGSdyymGmfwwljGJKegUjiRzT4CvigD0PXHJQkbcJg5wEZ3c3KmNSEs4RJE7XvEd20&X-Amz-Signature=8785f1867645a2716bc8d1b932f4da2ab160aa951fd447fd59a0f40f038bb7db&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

