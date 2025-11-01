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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WZW3FTOX%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJGMEQCICcdogTzK1PD%2FUqpSEbe0gEs6UEUfyqU6opetner9j4VAiAEJqnmwSHNgF10nTyPtH9ctI5BorXIJUNe9xnKWyFckir%2FAwg1EAAaDDYzNzQyMzE4MzgwNSIMWdkuM1bU74JzxKdZKtwDcRHyLx9tfGI33jsf5m0V2hoP5B2thheQ0YrtAAEzOg2D3JmXH3g5gGPKzH0ufocT%2FjRNU8vX1OHNrpORrkNi%2FuvOxfjUQ8iMH3Tjc6EwPukciXKM%2F%2FEyXeXNmXqDl7hrv5464bPPEvC713dgUOtaTBqQDqg%2BFc%2F4Vd21d6QnwqMxmw2dZdNmWULa7oULuPsDuEW2bdgf2cuIOqt6vZMc42YoJQNEfCd1ICK3K%2F99MKlS1RGmPAWfXDXQ%2BBLaKp%2BFr2Nrq9gP%2B5eHKbTxg6vfjj4YyVRKzIzOhnGeMex6uaUln8%2F2faoCgmXtjfArbygtir8mbf%2F6ivwuIelLQzdzJw18alPZFTRMIPjLhDeRwFacIdsy%2BtJV5Jeby2oJPZ9xJuWPYdoQPU1WAIoTt5W6N8Wt2G48xTWpvXajplV629P5aNKj6U9ayCe8sgqQGaiKMTVVO5m%2BgG70gAdIyt59wPEbSM8At2%2Be8LxQsfBKnQgucXapL54bSNNNdDP9vVENrGM1pumx2VJZFfC8aMO27EktivDFSIk7Cmjkg3H7QoEC9Ii2hv5bzHWVkXSj701%2BJYTtNhj4BVHLD2KIfI4NrQqcQDH7xf2fr9gWnLNgdRpt%2BS9hSO9ieru%2Faocw88KZyAY6pgHBOqiGF%2ByojmEykAy3ceGIYvhh0jMenSCE7lz3kLsXcuzfpRtbJtN9%2FMzIW5mrL%2FE69Bk%2Fih%2FZIdZdSLdtVpZ%2Bg%2FTzGg3TzFCnUbFnFw1M0WmwCP5E%2Bu8v05HX0mRy4e6bdSRcXCbxFFzjz9YLlfHB%2FfpAa1lAl3XGDyqYduGoKNFev%2BGNR6vw6hERObS0GFxZXSgd56CE1bLPsSI7xGLAp2rsUvH9&X-Amz-Signature=a0b8806a41f071e8113de0428c6555353c4fe6a9af6aa52e5d26bc3f74a1aecc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

