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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z7UTSFHL%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T130108Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIH0hw0MGMgA2a7q7%2FQlt3%2BgEPvSE66WfvzkVPEm7oNYfAiBDxWDnniASR8R7KqWXeL7c%2FCI4Zq1dMN7lE7A%2FLHQBaCqIBAiz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM0RjBdXM2j7OqfXV5KtwDGh8pZMXZC720APlhMtoW%2Bf5N%2FBoHEfEv6iwujV%2Btp8JHhbf1qDz0n95UiXZ7iVVNjuaQ4yyJTv0NtyXcrnqivuoVy3EWowRj1Nb5x%2FWRd64k8Rj9Lh0s%2FT6R3%2Fan1kOB%2F0tmaBnugzRYaGelaYzg4BgZpTt2%2FSiEOZOaU68bKtL9axtAOyjeCqfw%2BnTK2bmIdCnJ3GOu0prqyxKF64iAf0IUgGQbL47NGIdKztN8XFt%2FZlKFX8BTUQAARFL%2B9UULgTLr5mLMaPEUAAa1MfG2f0TUOVHQeqgVnXEoD1E9NuftwsExFyGk%2BLx7c9UYvvyILI2%2FImXEPa3izmqOY%2FhkGMzzApz6Xt%2FGcQkRwXpTMhdnXY3%2B0bZHbr2RKSIPG471uPcRr9BBWIq%2BUdoh2YnJjBYLWl849DJNNHfe5x1MZNDUufJmG38%2FijrVmhCoj%2FW%2BDuUleeh%2BjQ%2Fu6ASH9Ns%2Baa1aLAzesJ4DXMONEBuD9btZyYHy4dG5KpDMW9hQ8lGfA7fxEahoGmgwjCFb%2Bq1R6ZHEqJT1fIOPuhfmJu3ygoBmwQ1e16tsYzudVYX%2FEX3k3SCT0jA%2BZqP2bQo9Q1%2BAvBpC3nba3SegpQ%2BHfG7OcAYXtRhLt3jkEILQ4lAw0tilyQY6pgEHQjXRqKNeqL6w2ULhYJcDKQYrdwTwAYaqKrRZUS2IaBOwY8Y5yFZ94CHiTrIAny4elItvFEYo%2B74v%2BzEmc0THfvWZNfurUiXP8nB6miYpdgELgmGPtuGpyjcR8tXo9XDHF1ruN2WxU8OKkMES2P%2FqkR9jhKVZiWfBay%2BklLyNZKs1IzCVjuuyuxQCCnB9EcEjQ%2FjwFoyNA5CyXxtauteE2r5bCGue&X-Amz-Signature=97774243e8b0ce32f3aa11819c246c1dfbfd0b286810d894728847d3bed43527&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

