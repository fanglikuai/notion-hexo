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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662SC7FXUZ%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T100044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAW5NnIKBXo5Sqg33pEbBX60ZCIAPX%2BsWPHCScDJngIxAiEAt7hsX7Ou7cHz4QYuVlldIwahU4%2BMpzXImEWiNyelg9Uq%2FwMIWhAAGgw2Mzc0MjMxODM4MDUiDBzmmSOL6KfN0Nu0pCrcA5KE1yf6NaR8bSH6%2FDdf4WeRnaispuGs%2BxJZbWNTA8iG%2FQTIcj%2FeYU0gwbZxcQfIPVKOwjoFwRQKQbhWpSFOMKLgh9OhNZ6Rj1MbVaMZ9%2FymoFmSTnL7nc92KSWQLiGiGkVYtWufKvvoWj%2FRm5ZvpoiFTT%2FN3X7Uxq6OS2oaZjqIA9jI2LGxrGNS8Kko6nZwbnuaJAanqoQVUhn1OyplBQEupyngnu5EawS%2B%2BWR4gWAo%2FxQi%2BcnHdzsa57MnCPlH6zRi6kDUgZPNd9NaHp9xJhlD3t%2BwujymQbeVoyeWb3hSLieTHrKT9NJpy74SJvgwtUtuE3tPzmXVM4OGUv7m%2F5s%2FtspUUqiW3KUla0%2B1XFGfqeHfB5CV6sAan%2BBdecCmDOwE1N2KlarNpSn8QJzKnITN3OQOUYtxh9ClQTIAIpW28DqWC1CSRptgo%2F9slv6f9pTuQPV4iLKqdbl36ovf7VVfkIMBfvcWpVt8ugUmMmxSwGXlc5osUj4PHCWZgBo7OOKLZuY5wCH1r0Ju4XFhH%2BTmb71PBQdiiU8f7b52SZkhNfCxcRv%2FesH%2FNOuE6fasZdyCtiTgvQtv45uKLJ6MnbnUKFfkXjmRqwLFQgUUYXTyGqFj3uOXlku7d5d0MP2kuMcGOqUBCXa5h4OQffKbA2EYfgcLLEBSXqe2BlVSMLRhCd7OAGsFXoq2XbRUjFbbx9BNXdVyDlHfk5spslP0Gm3Gxk1adVbQFlXj9a%2FhByX29uAYGwBQA6gMA5KqtiHryGo%2F6ohQ9BR1kjhIeMhzEhRzZAe%2FuaTm9vIKRsGV543Rn2evKg9hJPUqmp6VVFYdZqaxIus12q1JyzXXfuhXiAqT9RRIlzOiYc6Z&X-Amz-Signature=370fdab389649c1a962d6746c06e3dc84916e9eb3872fd742bd5a25972482dd2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

