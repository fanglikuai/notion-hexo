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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UMDJ27RA%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T190037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD991i8FfC03nmPADTRRyM1H55wyWVeo1G9dJ%2F2IFgUXAIgVaaN4SywtJ5bVrM%2FKxTsTDyDSgERBeooP1FKK6hip70q%2FwMISxAAGgw2Mzc0MjMxODM4MDUiDF9g3J57ndoc8z43QircA33wxkgF2lEn4PhG0wSNc%2BFd6Wsq%2FG7FR6IaWrVp0o%2BrZopLkP52FFdhahicLmCvdk3H83VVPOZY5f44ZdRL9v%2BZ%2FmiLQrbC3it58M2DGNDj4QLBsGbQCIKnfnAWM9Q57ePai6QHwis6CwOPQL2kjuiiUxvxnqcrTscb8BgP3tX2TjtgArqtYYzeGomcI2XNQrABcD%2BuItvKjRAa%2FqIqKH8oOHlv3gTmKk8iSyE2qxLv8hhUuatEDzpUdTW5BinMk2DM4qVSJBj1b5Df73371ejpp%2F3WgU7bTpzVtav69Ud4JwJbM583ePCRVT9zgYph1h3i2vC1Vi9rIBh4ippM1SDcs0YtHRUcO0EkBeuqmN86qt2kDN0h9INPaL4nQECqXDY5OgXGkAKvvXysiSGwq3zkBJhTXgH9OOYK4L0OlawK1dIokVMkxm7UxJHy8oZtV5JHQLbc0xKKDpmHxtoDEpEjXUFjfVVMy9PG3Uxb4oZ1RDcIDPwimEjsg5sOawSQHWNcj9%2F6ZBFDuYq3PWv7ejKuWBuyGi4yNE%2BvytI3NHdZyI7jU2ebXmeZzIMpjI8zgXMjL7GDTMVXs2by01%2BaBAzy2Vh1hQbsp70sr0JLr0e4UOLa0gkTvYWkdm8uMJzEy8YGOqUBpjrpOHVXF7uV0ev3HiYpn3AJOSCdazIA2FSm%2FVopZN8k%2ByYlkww%2B0JmR60%2F10cceE7jY0gX8V%2Bffby6d1WcOQSwPAz8Q8f9h7vt3v%2BX2gq8%2BlquJatFlUw21qK0zx%2BGNK433DeYPWlZ%2BwphfNBVGjUOSX%2FVaXUyimIJp2T0nw%2FDzW%2BajZItPpIpzI1bQ7iEKPc6cZoeUEdHr8xbq%2Bdv2h3Ef0nmN&X-Amz-Signature=8b4fc9b58218d4906c0809890c3557a66d89bbfbfc809d35091ceba2bb131208&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

