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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WNBQD2C2%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T010053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGU8QH3v5BaJ%2F254Yzaq0IPpT6zq3vUjW7kNROTMQXWuAiEAn1TYROhDkTxbS8xChfmQKH5Qa9ji7BUe9D24L%2BLOdOUq%2FwMIfxAAGgw2Mzc0MjMxODM4MDUiDDv%2FAJP7mOtZ5wm4ECrcA3Mzzk0wRP8j5A02GMI%2F6I0yzYwqHXmYnqS5yB1ajNyVTigPeOzTYs3GZB9eZyYeAiS3yow8dokPDXoNX%2FruVHNLskZ7SV5g0MxTPQ4umrnXUuot3IO9ZNqf9Zw8Uj3nUkTpFFjA3HCWdKCJSqVcvO07Eg1v3IiEaxQVR03kTukYO%2BWI5blAC9mglSZEF8hfDWk%2BK%2FjRCzyVaDAb5Kaxz3LbSSzLEACcU9wzK8fRVCUy66HuYOWwtbR9gQkz80fVALWdYbC3Pa%2BaMuDyL07mYcw39%2FZ3vJkuYNTerpM%2B3LzlTxBcJQ7a2BDOK7n6WMFhpFxVE4fKAtLsZ25WAkGe%2BKN2PA8%2FYfk3FkbPKPkIYemg%2Ft28Vwirk%2FJnBfNnuo%2B2FB3mrbTnJZJ6%2FQ9ETHi1wZXgf4WXkbafXNsPe8Nbyw342Km%2BHeQLFhqEbgz%2B0jRm0Yt0Qsqox5enVllMuZ5G3ZA%2FqVA2QJJXpUlyaRiFsntK2k%2BQT3Nrsu%2FPk2DSKBMrE9QyEjos4MQetbAaV8%2FWyKCEmb4IeKXiLK1VaNjYRNZiRv3yKbQq6DJDgVoh7YUwLz6I9OYeGCumVCQxX9fPyyl8GwLCynZtL7d28vk2JIlnOzDiEdBabMGia1lzMP2Q9ccGOqUBUEngZPa1bwoSCJKVbykWV0bKRIXdswYLhBwD0bNYXchObuhdwczWSaXuLg4pOcWaY%2Fnm68evZ%2BTIlEmlTGnKksX6jtFhAUuIiP%2BajqiKImd1I58zgD2NSLqq7xiIlrXUFC3tMI21pI3ux7ANePtw0ayJFzg8gBdCmYjDBa3G0TWIWUZyqtVjX%2FusMoyHUTpjVBMcowXhIvSyt290KNczqYZrWn9Y&X-Amz-Signature=f05a7c5c9f25020130c6f1140cda974d06fd11da05796fdb5268f561f4a27e2c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

