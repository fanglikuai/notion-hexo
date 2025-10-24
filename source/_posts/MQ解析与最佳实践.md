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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VGWJRBT3%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T180045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC2TAzqfVSdEBC8j7u1gVbCTRZAeXxAHyfcEedePhqe8AIhANmKe9YPTCCMpQ0ipotcr1Ejkc0xxLMxw2qPgcDhgQfWKv8DCGIQABoMNjM3NDIzMTgzODA1IgwDIHNv1wvOhyXRXLgq3APm%2B73jZhbybwI8toH2XahdeE4oV%2Bg%2Fd3GXtqXCNwrTv2g%2FH3heu0FsRM914d5GrdvJfUPWJtJ9Pvib1Tjy%2BjAoxbg2gMjB2xyA11iGVWdyv2ZUrgm%2FYesZfyiw2dsOFmMmuPjC5HseIGtBt9v%2FKXgjI362ETRqRa6rCz8cBckSbHUPJfHOcAZxjDY85FMzklcsztaBMjLrgXFweFwJ1%2FuEe%2FH60aa8gawfWeaCuFDwnEWNej8qBS8LOKkbsttAdUMnviLEBHCRtGNMkccfJkeIxBf9UchQK3m6FNSxIUKp9PRgGUbc1WvdO0eCoxSSVVsyUvx7A%2FFtiontsvv5gYD22qLnQVAafY9viLVWb3zlymIuGECbTf5JT%2BQN0g2gma9PdI2CLIDaZuiEms%2B2xq%2Fem7if9ixmZqofkJdBk2yYcyi73ihTLCUSdKSKo9a5cQvzj%2BzgW9%2BWw%2ByxW%2BX8Ye8SAwRDHuuEh3b80cU2EqSpUOTVHN%2BGk%2BpccdI84Y4cF9lRQ85hpbusgnK%2Bm6beVH59NmKe6yydZHm4qluDBNukJQNw2yJrgYFJzC8TSJUYgiZJOcz%2FCawrb4rW1CrMBQqwWk9eoMKxCPj%2BSzrW7mgd8dpDrJ3jJu8yHuslVzDQ7O7HBjqkASb6I09Qcd2mJ%2FqZTx9UXP%2B6yruY3ZDCe3mWa83EQ9Rp2F3y6cKpAOPkxhzECw8Xs06Jed2pv7wTtgRqGUjrwuN%2FWwKL7n8o80IHjxM8FHa1L0JSWTmWogsb%2F27nQVXRYhAaMvfamrIGJwDqUduRdvUHkBgnf%2BWEU5Fg8NdjwX0gdRbWGFjUmnw2TFwuAIsisFQA6irLhRqv8JqI325TRE%2BDIlsD&X-Amz-Signature=b63368cad0b4d16273abebbc2963f677c79f0d204e073863b7b13e122f21a4cc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

