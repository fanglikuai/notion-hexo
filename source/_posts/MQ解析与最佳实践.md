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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SLPEH66L%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T070106Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDprYvx59Vj7b0gaZMq4ENc3IvtO8LZBucHVU4brKhihAiEA9UaN3uvYriIuyBDv4xyQBSqABWOv9Wd8RVSjHwpEfdgq%2FwMIcBAAGgw2Mzc0MjMxODM4MDUiDArfzUXnrc5WTgUlRSrcA3t0d06EDwuOTCscTRfdeY4D2WkAb5whjOH%2FlXQ%2B8ydtKoF7nmtjmBdlvudVczA5g%2FM%2FnaGMeLjukpaPx6HWy01Sp2yrmk8%2BDIzaatCVpg%2BvvyGtJKE1OoUzqXAnHfX3MsHZlU3kRRT%2BCTierY4%2FNds98BVnvvoImc%2B0hOxgl4ImYbJtWYhqvmUxpyNGbIOIXDG4CKLfeWj7BS%2FN8R2TXcFpD5n4IW6NAEKJoZKPIfLXoX2CTep%2By1QiDoOSXHf2bSIxoOEnZKlA8lJgv6s5xnquLOgAi4rj6vJXtP8pQNKEresUwtTpfdP1PhwcFOah7a7nbhHwA%2FMXjj0dvysYlgnqqMp0XHGWtaK7iZRLtwo%2BU54Dv5obxNJl4AeLcNlODRztrjRALfwWV%2BMtD54rFbQyuK2PvLKzlNuI2ZQNbaUCDfvCJE5t7qaxba%2FQTlvla1xttEISzIwQWzSIEI8IFvuiXLj6o1rCgnTvRJ5wh46DS8FXbDzi7N9rk7q2fYO7pk4FcmYBJTiAPVwBNTv0b7YzE5LG%2FV4Q98PWOWdLM6p3SJC4ouTDo5evMHiHs5mf6tiVNJukwFQiIlXTLOylKR93PtvGKcArEUyiEkFujveEXyeDAi2rlub56Sv3MPvq8ccGOqUBleANX2a%2FHTqWg4Qe9sVRKMLFYisMzyWpnE%2Faf8PLQ6RbhhQxV5VUuV%2FOlNKr%2FI9%2F8wnf4f5kpbw9r%2FdQ0FCBPENBfHEZEk2bAfW5nDkQz4jjM%2B3SO%2B8VlG3YCdziYhzxIOYZg8sio94ACzRWH8uSVngGL%2BE3373uNvOK1Ft7c4lv7OPO6et%2FgH%2BkbqvBXajR6pRpW9k32itrz9TzJ%2FFkoOmf1WLa&X-Amz-Signature=6c621015a501d1cadab506d3da6a7f32a5141ccc7b49c53881705d9005968b8e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

