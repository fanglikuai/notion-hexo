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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UP4ROHYB%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T200037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBMaCXVzLXdlc3QtMiJIMEYCIQDO2PrWiq8cMe7rE5YiUOWU%2FyGejWDhKIuVG3F4DZo6EgIhAKpx6o1Ap4748%2BUkC2hWx1CdQMYOVVP7t2hFdMvijznsKogECNz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwQuA2IqacGAYIx4iQq3AN2xKoOuVH3eIFfBd1l0vre%2FKxa2vPrQTlbPR1ItXEyAZsPFDu11BZBpqKgvIlcHSPZm%2BdSwKOb4UI4I56eCE1Z%2BcuWB460LSzEa8LlNPx6hDF7zF08ZgpCf7QuSUMK8gni%2Fr5BObXKLudw95qzFKQEXQxaN4N9%2Fkf6rDjjjPNmJiJh2uncDcZXJ49bm%2Fq8owm4qz4LT0THACJbs2%2F1JHjDgDAWKUQJd3sfAgCHtTZSH7fFZKWHCfOAmqnRzgOfzSVjOcPkuDtr2Q%2BqbGZzhb8rlj1myQ%2B%2FVJP2POrEmeOuPZd7WSaBzOT1nQWqIfi2MHLgebznyJFWpmAbyFSE99YdMD4adnrnmPIh8sGgZF7dAkny3tKFqycyzwHp1bRvqe7Sp1Ap6I6Hun%2BanMtpb40qIHzFyXiVW9woih9ruT56aMJZbjnhEmacNrPJ28zeRM2aR0yq69YkA7rWN1XsIWWctZu3GJROQTeeYotoSkldMPJ2uE9Ht6VpxjyR0YOuIVASzE0GVudcuOG1LjDAc%2B1feR9MfZ2TmCnPvDhBb0oiH1sbFgIkS0ejQG2aXUMsRjHeMzU2DwS%2Bc0806qSfrQh381Hl6lVPG9ccbWtC38hl9j0bHLre4%2F33niAZDjCSmr7IBjqkAWG1I9LXciKqlFHpt1Q9%2FXapRkXrDGcjnNhx%2F%2B3MegMjZqKBaEcaGQt5yrwL0IZ8h%2FB7z%2BEJEzRm6uOoQDDlzq4t%2FVuVKEWCqwg9LliRwI4CMRCS73ZVa43ob2Xelwk9nPhicy%2Fyl%2F5yNndaEDjhU7xrtMAW6smkS%2BMXA%2F46Vsie8aWm1lUijNtr3M%2BGsW%2FknKcRGNLSMoI716USkX4hyipLBcOt&X-Amz-Signature=a1ea16bc25c2b15a9db6d5f2877fa96382b56b0ef869262bbecf5e4df415efe3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

