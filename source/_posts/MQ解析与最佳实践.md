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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V6YKDKKB%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T100040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAIaCXVzLXdlc3QtMiJHMEUCIDBKOYPQ2gdPRurI7jBARQ5oR%2F2yFc7b7mezmMFNQ5W3AiEAwtUb0n5AWhoQlhL74Dobee6hplaOhyXXNn0ucbenpvgqiAQIi%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBThj0sZzanCid2FRSrcA58gcu8%2BIg7hcPEQf8o4Dc5kDqumhx01jC2Fg5DGznZLieZOPgclcecqOiJjjhl8H05Y2dwUBlvMzIbPmOj%2Fx12Ns950i%2Fz4zqLW5jYTK90qZSq0hzEBTQ3EDJbUEwBbFRZ2VK9GzPN4MVEXsYtECQqjYgWuBrlDT7gMrg9l96c%2FZT8GDtiZjKinYJxeWJOgXuW7vu4HdH5WrDaX3dFrN9Lb%2BMtTWv%2BMlvY35qePkM0rAmPQv9wiyvZSUTIeZqD%2Bz2TL7Y7ZMZ7zByPpcpMQBxFi75vYKcbMPOA00bmDHMYS0EKzJb1B1DNg%2FQWTJXn9qQlHMnZV1mof9i182qz%2FdbWBMNJSAIhwav2wDkxDsXWcbCCoASqQIAYDRmDKr6sPQJo6wQlIrckGa93FyZYt9jVfHiiK8elxnh5DeKj9zC44oSr6vFy9bWUPl5r0yi2AK8f6ofvJKtheE5ltgEYMIXALjflCOZO49IZCFm2zOQsXHrSBHmWle1OIU15yWwhW46kIKUg8SUJAW28I1%2F2n4%2BilBJXAdOo4nmuLonfyGD7BRwnw8L4ZY%2F5kdbjN1Qd3VFVExHWqJxCQPNl6ROT2rLSd1W%2BZ2pNCJhXCc2lL7YRVbJBkup%2Br9e9kq9jgMPi%2F2cYGOqUB0Bz4CEXqHJN%2BiReh%2FmEAi%2BMno8juvRW0UK49sMRpXFK7d4cO3XF2oIg10YO8ctlHw3i3HA%2BC4wcYX3IvEqyzRSHSC4NBdK6%2BqLFZejLowOzsW5YPNB4NywfZ5npS0YvrXId9wwaqBYBchfkKUHUINsOsEufg6ruTgIMFPwaWBKXsQZT%2FSeYfbRISWHi4vNcyK3o9oERqsnlKPaL%2FIr1l3hFt%2F%2FAm&X-Amz-Signature=7fe44303766b51ea6861af38ad5de75276ecb29c722b33b554145d6a6e52c660&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

