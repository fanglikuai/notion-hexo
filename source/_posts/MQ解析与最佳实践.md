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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667K75WGMX%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T070054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG4aCXVzLXdlc3QtMiJGMEQCIG6UZ7H%2FT9QekSQjY6rg%2FFQ%2FlBAnZaOgbLykqltit2jGAiBB2kZFDPdIlIOGZCkC309KB0RuuIcsclZGPVVojSqt4iqIBAjn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2FfXJ6uorrSpyKhysKtwDF4IX16dhcK9RK9ce56QyXSlzPj58mzIPQFIEk8RW5gjIbvqIZOsajLTar%2BdMfc9Trno2Z6%2F1NzOEa8v88dG1JfQJzwR%2BxyXm2paoBOGvU1YxvZSV8G6eYiLoMa5%2FC%2B2uzt%2BLVrTScljvpT%2BJunNJhBieaSDOFTIouk63On4mTiwHA81JB%2F6%2F15d8qvvL5l6YCkwbFTWFSkiW2U5SC8b07TAlNv70US3TFbR3z%2BYbSnywyZIv4bX8h%2FNeD3YjLuUEoWZkhQNR%2FFY1s%2BPVzbjiZE7tqvIaNCm0NzrFjK86cauzdRnzR%2B%2BegX9EQFblNd6c8Rve%2BKjBnrMTZfpoKYrkomhwLl8dArckvEg%2FsGlQn3iLw8Cwyno0eykH1ADKDh0fRy5Gqv7HYxvklNyLFxFMoRZYoJrVsHGzWic8hu8E%2BrPS%2FpqtYIocRZOkoIEvkdJNFlUHedwsm%2FKJnKTFioufvgrQwS9aIaw%2FH3gFs2JdBIsetTdh0tIJq2ORydSTSb1VMqpx0PARlKIpea77ab%2FCEj2oJTIX98gHb%2FbaexfOJ5ykgc0wimJgSL0lm4pYkTXuP36YypiHBOjbw9H%2FJ8F%2FS7I5qJSw3eVj6uSmkc2QZ5a8th82FBB%2Bj2mhM1owtIa5xgY6pgGKoFpYdBonDOvUcUeWGih2LWF4N7lzkAqHyquaZJYI9CpuNDtyoC%2Fh8DoUGSq2V00dzv2Wfe14TyeZNKmG1NViLS8MhnzOCxY6DNaxHg%2F4wMZVjX%2FJlbusQgp83s%2FwccDBA1j907Va7MDQ%2Bowa69Bq%2BeXzw1x81GTXmGYZsCC176Bk7s5vT1eBp0%2BXN%2BCrMff%2B8nWeukK5cNEm6jTtJoxMGIMC0Svi&X-Amz-Signature=c0b09990cc706d987aa6198a7d812536f1f32be02948ac8e88301ed15284555c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

