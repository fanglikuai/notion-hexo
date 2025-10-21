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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46622VF6CIO%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T020053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFEaCXVzLXdlc3QtMiJIMEYCIQDL%2F83ZyTM3xH0ocglp9mDPKykOnL19%2BOKKcoaVe0vlvgIhAKbSCtg08kvgb%2F4oWUNSBHdydEM7l9YoR0%2BMfnecVp2xKogECPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzBhZo6bDZpHWBLzcsq3APR1f2BhKsbogFfyluBHULkJZSLiCFrOtjHCvolKiu8hMLbdngcOZx%2F3eIpTXCWp07iDyJxZ%2BRp%2FoLOhzmY024B%2FzMiezlWRmbPxnYQq1F%2BWb9WWDB2TEvSVB2KL0Gw%2BcEitpiTICzCyq1SZMdndthB24H%2BSSsbnk4kDuQJymQ1orfn0cBFOAot2kJBF7MpxCjhY9KpSgUF9hHV8WXgV3KMnZWcBVp1nyFuDNPwiPVdIoaE92%2BV3UHgKEeZQqwYMwEo7VkeUpwS6%2FAVP5CYy52qWp78WN8oMKAcqZVoUX10UNmak%2FqV4zfc1bF6LBGJ7RBbqJoDrrnjcxtysL3MuDbTjq8q3mhQ4Rh7LxbkLJdkTKaigqXMTfg%2F2%2BS%2BkY7XOuUdFRH3XQweoIvM2tZ4SvyNwVnFmpxK4RPrtx3tulVOPBMd640E75Nun4cjMH0MVZtmb2xStBRFQ9sz%2By0HO3zdx0ng9%2FepBNuBR8qcfhJA9YLN9FsdPnQWpgr4CTg6f71wdEs3KAJsKjhKo%2FxYYVZil2KqN2lxFpcqpGnjEUcDpKKLribiXoNYWOaMMTEhJgUd27jZ%2FwOZAfn2t1oh2FoRqP0ks%2BmINLT1wQidB74wXbQidVtqyESeFGQaajCnutvHBjqkAbR7N%2FwsICLmFkn3jHmfnA4%2Fu1fxrqrLY8ePrHkCm5ztRNiVca3LW0vpKygHVVpchOD4oSE22GyIs4lq%2FBsWOgJFv1UI8jrroIAohWK6Q%2BQoFZzQEYkSORc%2FNFNHNNK%2FdL40DhaXaCVW3sZF%2BJnPDQYj0NBoc9D64cOgza3TToUM3izW%2F9ugRpQ%2FzzzvOGXr0B%2ByxnllEDhpqc%2B%2BUyQQ7c9KLim5&X-Amz-Signature=74cb648cb20c04b1a1016d5b4dd4d3f705a2b44a82e2fab4ae10308ab56001df&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

