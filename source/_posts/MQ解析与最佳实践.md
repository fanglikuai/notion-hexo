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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664DSSBPJ4%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T190140Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDCfey2JBuRZEBwjdS3vZJGSK0dWiSwD4vSgoY8neDI7wIgY2pgbABkdaf7pDQyqXEGOXA9EghTUo6GU5YcRoSBm1Aq%2FwMIYRAAGgw2Mzc0MjMxODM4MDUiDPxBoY5cI9aO4wc9rSrcA9SMaeqbdAQCJKvXoTiAuSrRnwwBSv2L0O%2BmPk3xHYhUbJoZnvy%2FucfSHTLBakdWEBP%2FrAeDVyd8yTUV2DJxWcCB86e5TeDmgrc%2BRK2sjOdSbCEyHewJ12QGq9SyQVizq6Xe%2B7UF2KU1YST6I724K4brUbA33Gi5UidGuBHHiwxOCQqSPgt7ktX2NPhvSGIbYE9DBhLAGZgIEshKj023KBrKr4T8WEDAFwBddFwGa3KfKPIf1yXZLmrKuU3UBHYpIcC34i3iixDVsrsXGejOjnWm0GSployoQTd3G8XTKh2p1aVfK61eZ7esFhzyX7xtJ8SHr7vVijL5D4gCZcmMDARaXct%2B1wvSaXqoJMhaD3P2tDdkJBuu66kh9pPo2V2qL6TsINWJ886dI6vWjNI2qj7Q0DYVSDSjedpCXmNggCxEjO57uix8KlE1AiY%2ByOuIldqm3OGczt0c7xmU02n4C1990p4NcJa8VzTFM8sFsk5FPvfLoa0AlRDklT982JdpWDy7nQxQqaDmn3Nqdh4prlwvGErogfwpgwJFiB%2Ffkdxi90EWZDkQDThLPGWdvv2MwqHM0LAicwsqOAwlSxhceH4UHvROdg7OYjDdOTT%2FLs6Ji4d5b0n%2BEXZSfKKdMMuPhccGOqUBM2npAw%2Fm3Yi4FOOCkGRC0FMgRogFt4Wad%2BBBb8WGK5NSmhmBFzsSxzCtmri7KwP5BigSbd98Y%2Bn6Q4qIL2vXpTvD6C4ACDi5X8x%2FTYAub5NyDct7P8fUzC7pMHewU5BxsSQdFpje9n6lIXm%2B%2FToa8dTCgM779d40tgGo9QRBPBSw8%2BCDfwVBtEaigj%2BKR4aeaYHtKPrnfrt1JMiVvAwYP%2FK2Adav&X-Amz-Signature=c349b117ebb5e2cfd7a89c64251f006c08c8ae97508d0c2a8ed791366825e67e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

