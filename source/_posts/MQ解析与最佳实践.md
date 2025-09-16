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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SFYUMYAX%2F20250916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250916T210042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBwaCXVzLXdlc3QtMiJHMEUCIBBO%2FFv3zMk77UHdzkRZeW6M56xe%2FAatPCKZsOnSUbE0AiEA75fDY9k5T%2FeaQ77a6sDrHJAyQXgxpAlV9Yqzljm3IJcqiAQIlf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDALCzlI0MsHvNWKtkSrcA623FaqXZzG5PS2zDBoYyyS9DHrPwh4ZcOT8vm0TU5cDYWNwGUu04v0rIqsPmsu8QXJhxUhyjaX0HrZD6vNiafJokM5KPjXfZFsD26TbMIXACtLbyl4naixlJA5MSDRxYRWLYObfBN5aPl0zSH7Ly%2BVsl5clkuPsF2s5LEgRFLuYy4BJ3pOqDm3M%2Bau%2BgxikqsKvfWSoQM4mQl90ct6w8P9zerqmjXJj0OOsbevkZQ2b27AowqbjdkOuMA4jsiRMMgEAUR4ifv4k%2BmC7ycwcJa0a7DTxGWELQADActXfGHkF1pT44Na4hK79ijBgvXzKKce4rorhHTEqhuoyws9d88V5EgfijSVKGxk7PZLW8Yj3lOOeSdNPOUyvcFcUDjUJFyi%2BntIkabjv5V%2FSYlgG0SzHtiPQzBAfj%2BlT1Ik1HQFL1aLQhjvcH5FDvQeGUaV8T3s4gcJ1xfuCUU36c8ZDRMK%2FDZvFHj0xhXP6XeWblsABIRlieDeOrnrVkXPKkKAfW2e5tuko2tLtgWH3Y8HB%2BOuoK6ml7XR73LZr1z77PzP2H8ppWvGyT0U20r26golcntYh6yQNmZFM%2BKKZ2xoRmjUircV3peC%2BxNhqq0snJBPdVzTBp9ChgXLmFzfDMP2Cp8YGOqUBgF6qu%2FHKBEmuR1Q8IqKGxt3k7vwXa80ca1mlkBVjs2hV66O4Eyb1ysRuUjjvE9R0DWnOXutdbMuqCg8E0ZXiz%2BEdYJ34G0qyC9tWpmww2mT78jLOvJBgETmVJ52yHDJcNJgJRz6%2Bsibs39q09BT8VUjo1zO3hmW4HbmPJdvnVScHX%2FWmz7pxW%2F4pmJSSE21TtFS66snvW0ZDrhtKqwR%2Bovah9MU1&X-Amz-Signature=a62da15b1fddafaeddc8b8e92e0b56de66b3c12eafa475edcdd9e47a84d0ffe0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

