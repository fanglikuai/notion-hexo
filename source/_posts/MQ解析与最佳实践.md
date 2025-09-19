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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SVGDGEWD%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T020148Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJGMEQCICWOqKNj2tvf8tkrn8RpP3oxyqGEddsbdFqCjkgMTEgBAiA%2FPVw%2BFCX9ReERd88ACppK7lUI4slhvYOmDpmc2n9muyqIBAjI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM1orxCkR8XnFoUmusKtwD7wI05W1b36lSuoIwlz3yD9vICVXsmtZFzSe0wPH4aQGg2bUnKwvCOKi%2Fl61z979%2FFak250X0ea1Njvml7TgCDhsmNZq4jSQnKchOU88K0icAem%2Fs7kZbIYkpdHJWFhLRdLtOUU5Qulc6SLY%2B3fl3KjG02zuTDwn2khRgH1KajiCTr%2BK4Q3uwe4%2FSutHn8VQcaI37UjVk4UXrCG9mrGKxDUue0hqK3LvfxwFSTxnBORwjHe%2B%2BZ%2BJpI3I7%2FrOJI1fmEcWAorlWl8dVygGJ6nlmdIxoNUY576LN0BeItC0o1stgdNOyO23XUFoYaYaHF%2BO85NnROrpzIMlUgM7%2FRd2qFdqnJX0uEhfWo%2BxLW32RBx%2Bwn%2F6rheGmgVP4MGLDX1jC3ysPapwUFrx3J8uSgCyP5fNkbkdxfP5kSQSBUqH0nM1wYQ%2BvgpO%2Fp3i8wq4YzvyN8PgWizQvtZ51sin%2FJ8b1y3c%2FslGRvHQp7WGGDI1g2LNp7UuoKIMKSpMYD19tlmadewQ5NvsEr%2FjyOYyHa%2FbwB7nmLW%2BLJTi5zXxV%2BTywDvyLgj%2BM3VDAF3c3xLdRvALlNWhNEshDUMQki3wJRFuR%2FdJH6N0IFyMgmAv%2F8998lb7CwyxUH7de7HTe0vIw8qSyxgY6pgG%2FcP4C5geOdrNMDQZaz7%2ByXrjXDe7purF%2FCar3kN00JfOrhIGK%2FDzfLEQL2nkmTwNYwKr9vBoCQXFfQneNkEW6FhSqqh7qYHmXd4PXGmuTT8MSjg3COcQ9FSHANirsfHRMV3kHQQJp%2FA0AREJ%2Fq9sviAcMjRFY40G558%2ByC5W7ekRcHlqIKjg4wHm%2BQfTWv9ZXA%2Ft4vqlkQBkXW%2F8PZcgUcx%2F6vjQz&X-Amz-Signature=b8f7d6988c04603cfc90d20ed02760f65f8d9557de4a7415ac6bccfe7329a833&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

