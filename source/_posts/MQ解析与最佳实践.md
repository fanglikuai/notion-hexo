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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TFNNDZTF%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T140047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDkqp1Pu%2BwW7765cdoLpx%2B9SsdAQtPEdtpuwa6sNaPIqAiEA845vFdr%2FFiiiMdupZhh5nosGo%2F0l0U15OGOqIBcAlgYq%2FwMIbxAAGgw2Mzc0MjMxODM4MDUiDBgwWNGTx3hothQF%2FCrcA0ykWPN0nb7EFcA0KNU3hOsJJpn552x6Vx84zN6fIDdhHhwwFS4B4yU04TmlXE6HmsIdEs%2BsSMSDx6Z87A1MTKjKEMWtsZmvkNnCC0fzEwT18KZvSmWDZbkUtpuIkAnQN6pCn08S3MXjGoJN9S9tZ%2BulVPzPmKplnPQ7GoVuImTffGq4T%2FZ2yoCeB0tWccIEmmNwUROuNBBDpzgyf7m0cLNkOokXRZu5649cgn7V8jGrTHY6q36ot85BwHtMnUq2sSuGlK3buNp2%2BYWmNqdV8rz%2Bu5vJycifFHQuS%2BV5jlJbY2hvNZe7WgOcYQNddXjh7sOxBfl64jHG6gEW1lY67UjnscHlucJd%2FrvvibLjPOSbaOXOmOmzAuQqLQUB0hK3O7ow4pi93HEpIZpBgwIv2exkYCP3m%2FkuBwiBGDstV95OyDEq2Egzg9SBprZvQoIVlKhB0y5IvkDfHsikF7i7A5g2OC2fojYDIy5RKDyiIezd6RGth7vajrnxCi%2BdmqLDNvqgyrGqigXXkQ%2BV2d5bWMx8TbfJ%2BZVnQdwAHhQiyyNUFG7AzFC%2F40%2BEvNpcvuW%2FG0lDZl4Wj%2BxeeaYjyril52HK2W3DrVDOgcERxkUnSrkQ65a8fdMfogUiIuUKMI3jlskGOqUBRm%2FDhDXsevevbZy%2FXZ%2Bp9cNRhccaa%2FomKXJmajxa1l0vndLrmdt4yw7K%2FSD3WatG2UU2AZURyZ5ZsKuJ6IyDX3IghUqd7w%2BXYZZGFOkD72COuwvXeY7mn9MGnegJyymRazZWqz5KngAlCEzshuljajbf4O%2Flo207jQyhRnd7TOy4WWkkruKxDhNiWs%2FDclllR8Y6GmIC6xEByakoKbXQoLri7Pmo&X-Amz-Signature=f648f8e22ad69858f6a1628ca868335a43b7db27225382d33cb5d5bb298ece02&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

