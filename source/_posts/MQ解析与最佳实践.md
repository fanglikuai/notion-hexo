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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RBRHIGWU%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T190041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDLNgICYSerIY3a7huxLKz1V2B554oLp0DF5cort9%2Fd%2BAIhANNzIT%2Ftbid75QQqlbD40ct5VUp22yxEunxbqQP0qUfLKv8DCEwQABoMNjM3NDIzMTgzODA1IgxSa3HSjE6%2FCErX4uEq3APLjmHZ4I%2BZZ7QKlcjBdaSzfuUv8yqpwCqjzln97JQhvkM4XrnLjerlst06JrDDDZjolB4JdI7VA9%2Fz83naCWv1PeTr%2BhJP5bKO9qR7mznL4OXAxYIq6e%2BUwH5evDkTWNOiQ2aqftR1M13rfErkE9yJ3EUcyXmpZaqeQBOfXL2oLdOKjJp4jJPilx6fPmZyEPrBjnviAfo6zkGvL1X99hQiErL6n%2FplyNNHxEvHT4gwTMxRnETZhfOWMHIK2eR42Gv%2F2WSPLuaktkb34shpGFnC%2Fqy52EXaFtjD9AjqfNvtEwvhsbvH7b40AUolfRdRgm3kgq%2Bh0sN%2BrS33W%2Bq06FGFsyj5Xn0Hj4mO2Ddj04Lfz7v1D%2BmS%2B3kzrfsPoLKdRCTV6C%2FH4jG26mk5nqrjtn4CN9Kot9QjtCK7g8vk%2FPNDtjzEp1j1S0kCf9ktbtoNe7T%2BaB0D6DM2%2BCNIFInLNxtSKp3aswf%2F89FzeoXUX%2FyqnMKw0qy4AXNOYSFo%2BOKkGrRWL8stXmKwBmQAU6AIMV3Sciow%2BX36CTXWwPLat7TreYTzwY3Oc7lzlNisGJKldsfTveAkX71n3iGJgw2F786IhU6zb8ZYIFetM7nMODEiEPK6UQyahSFm7zIZlzC3kLXHBjqkAfyKjG5RO3jTAfn0be00yuy4cF%2FFB78DrEtv4AMjdsJPB%2FWn5MPkn8PN2MGKSNYbGQhXrDzU5oB4LrnqxLJ%2B9v2ASwKJ5o1tM0hrcMgoGSkeHaFerLBgterqVGtOIh8VX%2BZDteSIpnP1GodwzljIrYa18cFYk4ROey6SLVDUv2LXTYsmJTREYdkO7Cqn9dHmx4fqSx0fYj1xuMf16RpOJlIs1dIJ&X-Amz-Signature=4a6591a525bfd39cb0cda1992b5d947b96307c777b8372a1773ccec944df2f48&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

