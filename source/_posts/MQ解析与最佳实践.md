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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XY4US3FP%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T190120Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBMaCXVzLXdlc3QtMiJHMEUCIE6NOnyVYmP5dTOe54z2P%2FRajl%2B3v4pvxM9V37a5mfZLAiEA6xOIlPHdBpp7Swl77NmQIhMn%2FkdyAMB92pYnvMPEfwoqiAQIrP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKIaag4lJF7IXY%2Ba4ircA4tc0Y7ABWY93oBaJqNbM28Vhh5E%2BE%2FQCaR6AXKxW9sBFa5hx%2FUQE6UENThN8fJnt7Z5fNV4NGE88%2FgPlx5aD3L%2B8dv5ccXfLui3QMUWGYZDQXdKCYJX0CzKdcGekJXufP4Xds3Ug4S4bZ0OAynx35Cd53HAVySl7mnKbar63p6dzsNt%2FdaD2dwZ8ZqNL3dxSgyuf%2FdrAxH0bgJgsMwD6wWYeSe2NrQlq0QQKwfMhWEhmAZtFb9ycJoN7UCvvNFMQciJl1EJgjidowE1ECE24UkJ2%2Fb1FerMU%2F5NlaxGvXKjUQasTt%2BLOKTYDAzUqY4GYbUEUtjaISmqD%2Fv0vVQyvMPiIKIRXvIRp1jzj55LuIw8roRs0qFSbMt%2BFBly64B49IPtaNHrVt%2BCvCGnMH%2FhgWZF7kPpNkVAIJtNXm%2F40W1c%2FVqp%2FL13pmna8v8H4o%2BGaALe5kwsPL%2BtcLP58nLFE%2F487zXEFchbXlChXBHsBkJkNYkaFKr9d%2B7hxz9xc6qTyjHfjyL8cWMVY2Duh8ss62Mq4vO%2FHeuNqbyKtTPJtWO%2BUFirD8feWLUdD7nUhU9Lnogr5%2BHZvmVZcD6ZBqJLQhf180wYB7CuLj%2FRpmSu7KwlUqS1R4BPjcOoxO7RMOXClccGOqUBwiWAIy%2FEO3I2%2BTKc9ePaWFqxCLevTiilFbps9yNjq1YX5FUZeMiOW7CajojDSxDqQ0ryaArcdgIDLIYV7mJIqlSifAnJwzzhzCCQi4jmeNChh1e3y%2F8fVWmtOACTawSGY%2Bvltsr3Xw6R2emHu9m2%2F9POhZOfTa0gQHJ8OO5vdIvV%2Fj422uCOKpztTJCUJ6lVRZeJevuc9a4%2B7%2FbB8wpfze20gbOQ&X-Amz-Signature=b2bf87e2ad50e187713939e82788c08fd5488c78498d093b2d8ebd8c57562f2d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

