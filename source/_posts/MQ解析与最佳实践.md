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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666PIL4PDA%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T080048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH8aCXVzLXdlc3QtMiJHMEUCIGqsv7UxopkPJdHPUfG3PWJL6SW4m5meTAL3CEPm1CwgAiEAprzKv0jnhVyeMlRvX7N%2FslEzoigI%2Bivlr9wn8WvJ7Icq%2FwMIRxAAGgw2Mzc0MjMxODM4MDUiDCmBp9sZa7GpA%2FpIZSrcAzdipK2kW06doZRY%2FHTJhk%2BTPSIZKJoAQrt0j6ww3utHGlBa5jGMoO%2BN%2F8OQuYh63kYGk8Bvy7JaL0oEkza%2FJAfBtbxh986yF0MgotNGOqvQmSBoxunvIRq8pFQCm7xrMzjhC5wVJN311p5KZWm8jQ0ULZ%2BlmomzT0iFnuD8I6z6VgNhUjaxL5EjajJW%2F9F2NarSQyMzVCcJNykbfh%2BHj46wAGspJYfUFrEslml%2F9XdoFBfGQWt5jzUfC0uYIIieVm4UnNgxzWXtgTiXD21fMIiTJuwhEhzBBlga50sKnGh0qwwXCIQiPcSYCtMmEoEHCGPjVWdPU%2FykZT0oVLz1MonjfDRaZhaa0XRX%2FOGuHIFOZzaYPuW23QXCJU6zy8eBO2FVTyHGEYvvx5%2Ft%2Bw6TCm3DcM%2FJW7wlBQ4u3H%2FLxCZz%2FSOYl0rxBkuCLEvz%2FpRY9FCFKj1M15a89zwXOzZaVnsA9cWjoHKtfWhDRIHzH%2FxJhujB4hGv8XDqnY%2BRkYaX5xm5ZxLx8%2BZzqCDOcF03OB6Inr%2F8hKD5ChjzS0by3oPugsrsfO54RkF0rqiM%2BLQqomwDbjAdVujznH6r%2BuoFKxHsGQJvn0LevXMg5OrJ2fujpw3xgZvZBg83H3eDMNjx1cgGOqUB7jmse8s3G1tIS%2BNLF0UHgcfv2EDr2b7YsDeZdZQiUh%2FJe6QIYyhO6WyaBbYz2AAi0BA2lchRuwbQp70d5354WOLPMsadCN%2FDnV%2FV4GS%2F1Lm%2Bfo6CNGTDH3qgW3oJwRuxASln5kmg3i0z2JXgHawkN86lj0ESJeP6auKJ%2Fc8RelCb%2Bu0emAklf3XMV88CO7cxIlL3Fc8cG4zpn91ed79fEu%2BhQEiV&X-Amz-Signature=c484c276de7cd2a140af1a1df863111dd232686b030f697cbf38b48fcb8ad28b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

