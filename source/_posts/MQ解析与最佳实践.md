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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YK7EC34I%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T210046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDwaCXVzLXdlc3QtMiJIMEYCIQDDkb59dCm5eCqFF1DTtlJuA8DRonJByjgooZy8eV6QtwIhAN9Yyxa5kryIWkEgnp4aLt2F2Pd%2BjPxoHp8A4%2BF825RDKogECPX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwGD1pZC5TbooYIh3oq3AOH5HXlXeI9YF7FwiNI1RM1Cg5WrmimFnWCpqcicBMUBibVFuZvsWItOVsZUkD0Pzp3L3Dx%2BmmXqF%2Bu84T7CwXh6kvc%2B5aP5oll%2FOCU1yCr%2FuJeMUrIlipoL42EzA7F%2Btlvvu4hHZgR3%2B%2B5SwbdfS%2FSBRZjX0tYFhH1ZJS5QKnsnqlXYokM2iPZJmjbmheTFNPWC1sSjjrC5ozui%2FmIdHhdYrei%2BNHFriQsjpkqmcMUU13mr2Y8%2BVRZFrYFlZhb%2F1MY4G2NOd%2Bxin5MKOs0qzu5tcFLrx0P%2B4rvTf8yVV7%2Fu8mdYwsHmJ5AoWsUV674jKu7SgNOQ9O6bNcP8fec64U5FZv8JtfgHT3DONmBJF8URdWHYcDZLyxFPyx0JvoEVss%2BXas1T13Xm50TacHYxM8jyPIZ9giRTXhnPsmg0PqkGpms7w9BOhxXX0WTNbfOuPg1e9Uj79qhmaGokJ3Huj1K9yAN0MyrHszydwVhDJrwQQYw8Lu1HALcGViuvf%2FeCIPd86fkbE8m3O0pYBU5YWTTfpwsvcnOfljRTUCYS2SY%2F3kFIYVFnAn%2BtTeHl08EJV1JdDQ1xDVDlk9YC2tolgUc2b%2FE9f9JANPP7YRUg6o5d505V7uFPSrf37aHZTD3jY%2FIBjqkARiHTMiSmDeVyRNOKnoJA5I7dj%2BBBDx7tsm9FuQQVeflfWO%2BapcTtv0TecOhhcroyl6iWY6iiCKejGL%2F2NTW8Qf1MTW98RQ5stbL0Je3B8vnUjijeEYwS4pM15gHAOMsTTE6hXgJUkjwEI89aUSqY3kY%2Bxr1PdvXjODXF7Pwg1Bp5IunECFoa%2BlGTFfICl7X98iY1zP8AqZQ%2BbsxOIjPQjnVTVaX&X-Amz-Signature=3f82512f73b3e827a33f5a82526c2bcf125a31f69a441c785e1f92dcc4caf85a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

