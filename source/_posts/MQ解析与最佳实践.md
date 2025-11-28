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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XCHRUXDO%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T000047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCtVT0uRz8wyAZ7hwnFS1ig27Y11BKMWPskEBWgu53C5gIhAK7EEoxwACM3LFXx4PUzb6gbLjRhkt89jwavJTJVrP8uKogECKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy1%2FH1hc6LoGqqaFsUq3APIzP7k9SGmbwwJy6O5rPUfZn1lU8T%2FExZmmVg%2Ft4Ni5W3AVjixKjAJZXeqR0839xBmqBEBto3VjioRnV%2B13b4wxZrHFcPLFXyA12EK6%2Bf5U9rd6a0S0Ii%2F8lMdYYib6%2FOXAdXWvU3xMbFk7yILEML8psa8fnrDb%2B%2FRgpFLdJLIiUHRbtgH95DtHhVZpQnkAWGd9C4ysV%2FCM1m7poT9lVfZsyM8R6OSEJq%2BcStgBAgl5zJIaqUtv5l4LGHejg3gMi0o9xLo4q5bjGKY8Bbwp8AHQvoagprWSATGqoy5%2BViadrJIhqZTclurnyn4ll3ZDJ%2BXRnq8K4VqunPWBd6ORmIImSR7LZsigdQtMjhI6hbj8zLuVKcfW4gQw9M8ycZv0tzqUm%2FWAvrExhhQN%2F6Eoa98NwzE%2BUSwX4PcDrLDRRP%2BaTgZHnBxsUkUywOtmP%2BYpIWtjjL96NTIL1tBNzTn8%2FgxBKuVkggtbDkvIvnmdaOsJbEPJ38jdaukKpk1lqwki9oDpl5iH96ZNInpfudvAzPxaUhVoa28d8gwazwPamdAH7%2BnLnUZu6qx%2BOzUd2dvqf%2B%2FZWjtbfuxUHWtCvYi9kwlXoVkjdWRqXuEhOq%2FCkK%2Ff3elyRwgMmVWFC%2B7ezCswaLJBjqkAa5rhqhppyK3vsJD4%2FO1DAgOd%2B6d01Dbe7OjUcCpiK818InCzDFNb1kT1oTHe0m%2F6kMXwjeI0BdLmTBZ1VqvA1g0dgYSTmpR0MMFAREv8bY%2F0AegztzHx53se4fGZPOEnAL5Emhiqq37gkt5Y0DnahGQHKvEkCeJk%2ByyJTnTn6zehKCZBvBgT0FbvtcdvuPfn8bWOj6YHqkhN4ecv6NqwtkkBSeg&X-Amz-Signature=ef5f96599face2f9109429bbabcbe0ecc162891acbbdeac7a50fd706f4f0ac14&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

