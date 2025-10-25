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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WWW6V5JM%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T090054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDpUOCIDklE9vA3FYSCiDzV8gE%2BGI%2FErRfdBPKMKCGtzgIgFUjOeBXpR8sOS8Lk3%2Beo%2Fkts%2F9Q8aAxb8UwWRbB78J0q%2FwMIcBAAGgw2Mzc0MjMxODM4MDUiDLHkC56yH63mu2y74CrcA7x3ldH8iD4YmT0da1l1n72fOXFviRQVB5fX3UEnlGTAnfQkS%2FvMc%2BFznklvZ14xo9Yjr7CWr8KYtPu4jToX2Jf8K%2FaNG23R7D7FseK7Q6wONPXaCFt9xOJCGxRqiGzjbc3DfMN2iu7hXn5qHni6lWcVd8cHkJPEU7Dfzuq4SOUUvI2ZnthmGWjUzdqDrH6QigqyyHJVDiwN3%2BUnuhBw%2FfOJCciMvgzSKlyjZb8Qnze%2B5LYb7af7pRJ0qKFOTTUgWI3d5vEaB%2Bdvfas7%2BqRwtNxGL1M87BcMqrIcfofWrbE0XSx%2Bk0vvJMOD3b7%2FE%2BsWvAx0B600p22Co58BVVbixrHfBV%2BKtZYLG5I22xnKqCXFw4M96jTdIh22GnR5K3su3qqjg8kFYXQw5YFHWMaU0%2BVIuqITFdbm2uSFGbODoee84MWuuNdu3WTd1Hs7w1odQEOerMog8SeOgj8owcso2ZHm6sAnlkmqpC86F7PYffftGJT0CWDQIXJSzhqOUXtdfM1ug8N%2F7UXkzdyv7UlpEZiCj4JM9zdFlnMy6oixy4RrsP19koaoIqXsn7yvqjxRo%2FXaawIPBjO2xBg423ZCzTiQvj08qPrRPqffviFAZhr76wANK%2BUuSwqwHmOLMLfq8ccGOqUBwmAmy6NQ1QK3oCtmmgRvrczRjutISnBwqr%2Fpc0zR81VV8YlCk9Ywb9NsLcAYY5PNorkxiKPsHnnue8HTTZboUFyhOWkiHbe4HLdeYiF1ErKSK76%2FNu7VO1Q39nh4pvSHcgjA5LdLqzi4HwZarXfbrsMZi0Fg16oFUcPzW8lCmoKy4CsF5dmA9AEguUwIsr9jkjjwE7wvfRE2pRHKpfn7AZSsxBcB&X-Amz-Signature=9340cf1a712cdaa1da2bbb30caecf8dab817a16bfb585c5cf95d7fdf57d2997f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

