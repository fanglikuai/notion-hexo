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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QSDNNWZD%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T080115Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJGMEQCIG4q1BT3AGVqLsGPQcnTDXXpJ5DiH4uEXi82sdbTnZMKAiAhKVB5H9DES83FW7u4HSdh%2B3uKt%2F%2BT%2BmRZmQib3XGJoiqIBAjo%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM7Nlyg1akrzgS3358KtwDXCRn6DtvQkOcM%2BnshhBK429eCsuuSPX0GmUakEp0jyO00LXLJB6f%2BW5E%2BKWhelLYfa7var7xDwXaaMBqzfKu%2BfEb%2F4oVmQ9%2F%2FVekfhjf9oQ8n12Vyy2X15GLiIHtqSZ13Mecz5Odl%2FIJ10HxChs9cZ9MwDhgcU1CQFBP%2BbJbPuuorddgoh%2F%2FHzHvbBY5j8q8TEnubLi67kDpFsT4DkRaJGuiVffl2AIl7Hv%2FxmWYCw7XdwEWB7bYZ7WN%2BGH0PlCPy2pTDSSJHBKLgb1rYX%2F7fsuI5n9A3ybVcRDvR3bO88YVBB6vBu83oPe%2FvEBL97H%2B%2ByjvncYrKNjGz0731rSBG1BlwKY2MsUdNkN3mcrnKkdEZNQhQu2V04QOJRr9tcpHZ5wQjtIG6mVN60P7Zn3wyOum0uQbFkGvCiExK3BdP2B9MvX2ugk4FF6criysHCS0ZN%2Bnc0%2BwH8itrP191XannK%2BV5e2l2oNrApK0aoBoefoPO8uu8l1bSNGFBmW%2Fx7PU0cGGRX%2F1aD2etwPmEO8NXADAARkwfVK0wEYtEuVD7KOZGpdYdvZLR2kGtIvhirrypmYdiQHr2glKu72ddNuVO3fVHTojMrk7yWR6MYXZDClYk7zN0Bd0YSdrrDUwqdqixwY6pgEqTuApL9B6KV51RJIWzwsHMOjkErtgAG%2F9gIVMO9byujdo7piEGwyxgyJxIQ%2F4aAPYD8lSb06PG96SfWATyyO9ky8YWTfbngHe%2FZZti0pElQT3UNWoBp4mnSqGqbniaXz8%2FrsAfSxTcRUiaxE5AeVdmfZdV6GeKavlC%2F4FK1luWcXhJ5FS6rz9uvFGf6X1uv8ABV%2BjohLDl6Zz36gHgpwMgWRy1eL3&X-Amz-Signature=b0bb77420daebb5751aaba5b5167fdb25beefedf23cde947c27e490ae94a3846&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

