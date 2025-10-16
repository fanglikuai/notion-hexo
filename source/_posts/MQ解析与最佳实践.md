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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZZPTKGLK%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T220041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIG7AYbbNFKU5foKj82GNW7TovTKUoK2dMnBh68479EFEAiBMF9jwZbfOG2Ohdljn2v89UcKUltg67BvjSrsHcp%2FJFiqIBAiW%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMolD%2BKdyAp7NTb4TEKtwDhpjRdWimelQyzP0mBVt6JekfFmILPVCYin9Hfb1oYKsH5xFvQSUiRsqOAKlPzHdoNaG%2BcnxxoGLhFU3FW6FI2ZDzGDGfhtLUx82vssswtbtpkvJc2xKKq6GBt36xa%2FD3oIe0w7Q%2BibpXaOYza%2FBGROc9CZEJd2kUEWStl5tadcbhLLpB0o6len%2BdZMbCkU3O47HQ%2BbqL%2BeAehV%2B22HyftJxdzJ%2F390pUTRDFeW40BjOX3k95Zzb8adk8LPgLb%2FI8IIC5Nvg4no2z%2FIGHPAGxKX%2B%2BA3XmFrgSmJE6F7%2F%2FpcWqptVtnvCZlyGifhbn1a9HgF3H%2BMMuJyd3TJ2TirUrtIg5n8U4x%2BgjEvtzKKBz2SfnXB6a9MJOIfvP6s8obOqKaSfBQ0xp1CGEPN2asVaEjH%2BNgHOcj3kPBgs3vl%2BQ7KlTQP%2BNT%2BuXVFxIoFblAsDpzFwUuhTsLjVPYUhDJ3G9ONipYOC2ZNimEhoRiwyYvlv%2BxoBO0sQVgb77gGIuR50BIW3Tu%2BgOJ3b6UUeXdhvR7lGNcJ9qdWJxrj4ah5EodYXEzXE13Yf8wY1g9MXETw%2Fwtp6%2BV%2FGxudrdbgi6qwF%2FMIsKurKytYFruA4%2FMEDLlUl2%2FmJ3yvT6y7zfWhEwm7jFxwY6pgGBzwTz%2BE5WUPsXRbWXrlLByCNTM8cqpvP1mFKbpSR077Or%2BSA%2B2Pck1Hx4aprwgNCBYHRI%2FEd%2Fq638UaJRdpKanFf31cystd%2FdABFw6w5aiHJIms7bkMVnpPy%2Bnox6eJ1VUGkEt8sp%2BagtUurt5BuRjsr%2F3ZC3PnfzC23LduEDadB6WChKvwrAZDTqpivlsTNpDeAp5%2BZsPT5RF%2FgzNoZTRqL2KI4a&X-Amz-Signature=8973a70ca05a582262b9eadc1f530706913f3f6b133d40eeeddfed5b6e0cdf0d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

