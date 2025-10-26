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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XAVU43BX%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T050055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAU9M%2Fyt9wJ%2BkRGyD4wphAIICg8Rh0XTyjPzGjpu1YlPAiEAsP%2B9Cece%2FXcqWQQ4S7ycCu63GIA0C%2FoqPIBBkGYbSn0qiAQIgv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGEWiH6lg5FjMpGpfSrcAzuDhYCTQIf9kxeespYdk4gM%2BOqVoaSrSNb6RVn8p1kWz81VZ2YaDkv2lDLWDLjjfBRc93t%2FvjFIi%2B%2BMtOEVewazfJ%2BFUudRZ6Z56TQfmJzOKWdswomJrYsdcBMwN3wkT50IPqV%2FX5asdQhtobbdNG8aqO0JXbqRw41AbQWcntuxTJcQXf5dKrM270mH4piyqafhIVd6Gex7PzwSy8Zqt8QVCu2HUTgh750Rd9ucKzj9%2BkFwGtoi%2BFr5fwU7schoTsqtI2vGgI6h6KqAi5lCrJibtrr6zcJ9CmYSi%2Bjomf8X2xlA5%2Fo6hUGFgpg4%2Bpqu6MjRMdUGorjGK%2BK45u6D6gvC%2FTmFeueHVnKmppwDHzXtI4SQSdEUGxg6cr%2FEdtjUszjyW0P5bQxrKBtorkuTpq%2Fz0AwkKfBbE%2B8MoqDmF6DuQU8LO8zXAYEoA9QkV31At1tTOwo0wnnqQxXXT0zOhb8PunxXk9816PS7w6cPaoUHWD5Pf3bzamxdrTBqSWgVtDAy1zEjV6iORZc2PQpRYZRDIUhmkucno7RhY0serzn4wbb5akjLeiDf9c4qZQzbWgcqtLnk6PjKf09sdDQeZynvTDWwr6Xj9M4z8HR1AT%2FL0SkBMq0mmhCpDvAkMKLw9ccGOqUBdY%2FLjSFPoRuqObo0N7M9KgeMfi1uBpmB1sm0vshJ32zXaFrOKhZXQ10IWuTh1SXhiL9pU1r%2FJPB7WTrIBEblKz90EU4jk%2BufEwIcKPz3GZuzpgzT4T1ksBwL0se9twwAAc4irucltiqwYdYVAHCCEOtLix8dpp7Hz0lhfUXbMGe19n5AsFG%2F3a%2BZzrf4wSYYPu3Y05DmkXoFeENnVaS7Z%2FINYA3y&X-Amz-Signature=cab54fcf6d88f2ccdcc6a12cc3d6e192bbcf57f043a17da0e2aa9d40ddc25304&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

