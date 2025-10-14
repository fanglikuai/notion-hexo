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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YUQX6LS2%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T080039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDvK%2FQJkdkyRnjCmalLU2UXwgnmw4DZvUGxAJ3%2Bxwc44wIgdYOanPIxUppgoue13dPK%2BLn8TKX22KYse0GrhcM%2B91Qq%2FwMIWRAAGgw2Mzc0MjMxODM4MDUiDNHTfIAK2tORVpHDHircA4laScqPOVFbjgc5oDjYfgngTwQ0%2BeIUMJ%2BdA%2BZUCTunNndDoyzg2FLcDLyDNLUT%2BSvxRS943KP46Fk0uZ2cThKKbF%2FHV7yhTZxAjgxjzGqwx5QgFkuPCEKbc3ZJuYC4edxEeyj%2FJsEQEO3mR%2BeABnDu%2FSldDC5Eg%2F6XeW4sqqFlNF9DK3jCKB4uVsPa4PA5u87J4pVkW3jqJ0StiVxLsA9tH1egj6OAG52MXx%2FpS84B5liKqQVJ7JfYuQ0%2BFBFvp2ysIytjrnW7%2FHMZJRvjdG0wUdRkzdojv34FbMsbByuUsR5xv64Ys3DjG2O3DmdNYNfEaJhr6fF5lffPNc4cS6e%2BRkqfnLuA1EMwCnDnMyNdP%2FfbtFf%2FXPtZToPx7%2BX5en4AKjMlyWUtqOMViv4nIkA3rLXqbqhKH0q5dAAkhBw%2BkjdKPxnloVJgbVmESw9ahtQZ%2BQfFhzoA7Viun3WSpg5O3AMfgHxaeHQFrVVLeTv1y9rWyLm65OgMtAezPpfb7%2BAZ9%2BpFa4vYWggjj65DfSPpZFCrBkxcSI8Pbj9cccHobWMr%2FFrgrRPZmklApyCe1C5xcXb5C8yKnpinD%2BvpfUEVC5BZTys6D%2FdD9NB9wdOKLh9V41QB%2BSYbKCXtMOeAuMcGOqUBzdEdVQAhvqgNbbpN%2Fd4PR0SlN%2FdThEalR6Pe7ieWUnEd%2F3NJhg4rWsFuoBSKI5WTgeWtk5KBVIDPzjKGtmHV%2F5y8oozVdi2U52KoEW1CsIcNjU00ArQbw%2BOdfjWBi4G7dNB9L%2FlBI8NwinQQsXQIhUxmyuJso3o6m8cTUYvZCHjn9rIURJEwGbOZKPMgZ9%2BF7phN4dcAaEfdZALpYwTu8N9U0mFx&X-Amz-Signature=2cfb75e2b2139e43a52ec730504980e4adf53b1221e90f07c45ee0e147597c7c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

