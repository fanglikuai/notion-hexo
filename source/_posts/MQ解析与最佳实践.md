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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QYNPG66P%2F20250915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250915T020003Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCjZ33VKvxapVpUM3%2F39%2BEAwzlNdAWkv2Tdkmzc5Xi0RgIhAIljVwU1npn2GHmJOwvDzK9GmHi2J34z26c%2BXi%2Fmp9i%2BKv8DCGgQABoMNjM3NDIzMTgzODA1IgzEFsj%2FJ2Z6giPH1nkq3APMMOU6x8ujvqxyQXo6fcgO%2FUp9XaZcxFRiWJgC%2BaoIxiIaJ7YM8aRUp9hlFMixC0iHn8p1HlpfVqopmQ3KTNlCHLgrbMR6VpE3q0rHKIUDuviNUlTvNwZ1Av8M7f%2BHaKfeokXCzYZLVdFmolrxJFhdvpySrICnMHqGw1CJElanSvd0%2FqMCwKGogc3s1cgkzyCuovxa0Zr3jULKWP9hWmDFP44tErJUt4bB%2FpGl3bEJdNlBdfq5pnPkoo4W5a5V%2F8p9Wlr9B5IWHjpzb4kFFo0jT8429jMTIfXj7lnAXZq0cGLjsM5eSD2BJx2F6LHb5u7K63%2FBeRf7nBtgPrVH%2FE3IKDtYFLXnr3BCZKDqC0pkC36IOwR6Ep4gZH8K1nwWxssZEaCw5pnLFVr23rpIk%2FWumGWP04fc2G9xJABT5myzG4rpoWzJcivEaUKi1PkY8oCkqGFtf5M02NRKZS1I6hBK8sPj%2FHxLKpYSzmzU5PAZWv713orxTwll8QV9x8bgJhDNU8kWIL62%2FyKu57552fe4akbQ3qnHYMy91OQw6yS4xI%2BF8bBxI6kY4LYD7InWma2A%2Ftv4FZdFbat4kZpWE27ApkJ%2BwcBHM8%2FVbsgA5iw5p0CQbbVvj%2FHbRygqaTDsnJ3GBjqkAfgRo1xKZ9%2FSEj1%2B22IA5YjsA5IEO5mohB%2BdzyaQGKAqNTQtElsOyQ1EkNin%2BFrIwdJf8EAYsyIwRU9qV%2FeNR0cLcwktg1MCkg3X1sKSZYPt1DLme5M%2BMTykQYUo8AyAVmuW1DrngQQ7GkKnScowh%2FPGRkLYn7l%2F9m5Nez3O7vDS4E%2B7UpTKYbD6A9wWbiyzEY%2BhR8BhuziNQvlabwzZkmWBGWTr&X-Amz-Signature=9be3e78205b34f84642044e36fa5efa83b225a2cfe397acc32d343c2620f1de5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

