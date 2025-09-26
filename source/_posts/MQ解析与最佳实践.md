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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U5EHGZP7%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T040040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEhnuyX5I3VvXEPj6MMs2JMrsmeBEvI5H74hGgLdZmSKAiEA5BXoS3rahoGckWaEbFiSDWqtgAcHJKFuzdtJ5KwxyrcqiAQIhf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDffmjDKqO6PnPuBYSrcAxnlpAwPevDsHEmuVjpXmcJhqWiqB6zLDmNQ%2BOsPhndDf5v4x1ucVeSzAcYLXxMxvAXyMB8t0Z2tED8ahFzQYkRjx4T%2BTWRovhOCy0jS%2BMKlrUKKcI640LtJEhsAEILa%2F6IJbCN1LkCo8CEaKWQVu6eD0B8fbkEuRDs8vxWBGUxPXmfN97gTApQ%2FrLyd8KeKScpOE9XnjJysG8PoOHQt0SwpWMqlXeW0wZzyTdnKwcZ%2B2u2cILedQ1ecGpdVgbxx4JmvI8MS4LFluRzh%2FJXedC37KGCYXRYqFLiXzmXizOqxUklN5pu6bmFyYTIvqMkWtjsnif4n8OtPb5ECzkeydg6c6i5zbMzktTvRL6%2FqyrASpbYuqGfBnS1JEMHN7q%2Flb58wDgoirzO%2BGAN68uqmWFcqOIygABBFiihLEGWzbYNWiNI1disLCLLWsjE2WXWtLjGtVJ%2F5ieP6SsQlrbp5C%2BhtoRtsvhZQwtTXnoiyqzxCjrXnm%2FCkviKtWHA0F4mjDjteJKXsFvOYH6Vz5E14Md2rSQCm%2F3XnpvScE6ZnOE3tZ7XT2Dwv0XDeY9oMz2vx08RPIo4KVlN9IGZVw5EgBUgjWSbcFjYmhXQsdawX3I0lLV0RoZ%2BIDE0aK6kdMP%2Bd2MYGOqUBg%2BO3E5P9csDiLLjP3O33QHycWkPLaysVMjRj%2BSJjFME3o4V%2BRt5uEjE5YLCroD9%2BNkqgnrWBW0kuzSqqHJBRZe22hzu%2Fv8Q0MXxTQyu%2F%2Fth7G20vIVb7TkYGoSjI15D%2FUL7X1WYYZualAmca6xkNAJJxIRB3pn64rABVi9i8D6Ug4IFhl7BGmeNpg2YAyDFdrqDw4gGaBV5PwqOakJtzbAJb6NXv&X-Amz-Signature=2fc5d247c2d8450a5a5735d212dad8b0e6debcf937887c8c082597ceca7a40b1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

