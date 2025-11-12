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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SNRQKI6K%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T220037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJHMEUCIFjp%2Bic6SNt65bCR0DaME0DS5fvVEJPXNyzSinIA4VDRAiEAm3rdr9S%2FkBrQ%2B40HY%2FGWpeHPq977btqyhjRelCPh5wAq%2FwMIPxAAGgw2Mzc0MjMxODM4MDUiDB6OTvD9%2BrFIO1DW4SrcA12OXcrs06fQ%2BztJMyvnZ3FwOcmY%2Bvzk3bl9v8kTFHgw5OsmDsqCf%2FZunQTpN8Pvbg6nPxs4OwYJR249FI2CnUkg5GoNBR4%2Bd8wXBExTlt5XPGqgsCIdUTaPwkzIrLEBjkpKTH4CZh5BghS%2FivN%2B83mFHRnsYIIlvoSNS25GkG8ORHvtw4l%2BFLVEn9fIlIhtZyv1mHKrh8zke1CxXSe0%2B%2FlbmJtwoASRDKkU0ZMws7KuGUW8MPu7QmRVVWApKf0imEgTRxMYZiWbtezkWMPxsumvGaBcBO%2BQlEIRh8wW1BBQba1yLZ3zq0LWcwsU2gtM%2BO15w5Q3ij%2B10S9AmR3jjEuHo4kmLNJW7r1C19mYay5%2FU6gOc%2BWKMf2qOAqbZuom8jf1FnHcPRTBE8a9KZZc3Kc1lnLTF6T8sh%2FtTzcRtc3M%2BDqAUSaPEbOY13a2saOfIgNVA71MI5LsmYl9MOZ8mm30GWN0PRk5Og5E3ddLwddxMIZzs8OHk2L%2FTmdO6KLc7wGBqT2ogKJ1VRUVM3XOwREV1JgwJ%2FwKZ6f4oXyiqKuAoce8ur9BRwyIKENsHod4Z83mEPlOTJ5wh7NBQaiRTrNlRluevBfuAFPQsYm0MVEifYgQlpphyvzAK2bbMI%2F%2F08gGOqUBoUVHejn144w4pQ2qqYMv1QsyTnNnhJm%2Fig2dQk9U778bf7u%2BnCafNi%2BOJNSr9TuPZz2OT6hCBoH4oPGhNLEe6246oHPhsD52kmdCf7kDU3hcQVTZF%2FCxIMflOwSDaUUugJU2hqtXFb3RWvg53skq4JOuViUj%2Fs9yTwhOH%2FA5142wuUrDTW918zYMw9fX0pphMcFj%2FNj2%2FmxOHI9kwWWMRuj1pkYK&X-Amz-Signature=b137bb5233295b95f316903ae61c8f505d341edaeb91fa416726c55e4fc6dfd0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

