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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46664OQOGEE%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T190037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGkaCXVzLXdlc3QtMiJHMEUCIQDEkghuyXG9QlHgyUGl4KPdZqDsCwC8YrfydbU5xtAlhgIgZl2hHrrKhXyR3LtiGmcD8D%2BSMnDYUoqHMkYb4neRdScq%2FwMIMhAAGgw2Mzc0MjMxODM4MDUiDItgywClHzsh051LNyrcA0Lm1mDkaIn7BF6TjcKS8k8zofgkEqYsmETdM%2FoxI9By1FkBNbJGEjAt43lB6OgbArB5yiXg6847KtbN7Cn9b6iGQ5pdwRlBqFgxZ9pFdiic7Ab1E72%2Fxuch8uOQBeUS7ZOmqjBrrUzgU0NP5B6D6VEJZQTa6qdUUMZDJ%2Bp9mHxFjLl10A93GHZ18ViVs98FII9v92uzHIlmq9LlAtZlCBch4ixUvSqcxAVivzQbnwBWcwfI1TlMyo8fpNkVzfAlPYpJYjDWoZ5Gfffv6HNpXao%2FYEW6sG2qOvuV06T4JoYKDK9u3HNVU8Ejbs1zMILR9ayhee3ApFa2FXMwxkUDxHfmKQiL33bA7utEZaAW459Zx45Mi8L6HKDsMrqMfhtpKeiy8Me5rcENBurZ8NH4i1Td8vlHc5rk3bfwS4BxNBbF9snJvKjpvYTk4BmPE2YMX7axlIxfig8ajN%2BDTT%2FLdXtO3HIy%2F8wSVnMw4ftWdS76Pcd6yqlKQK7BC2pKHwPjlB2sSAuBWBfvJtpl%2FNk6WzcpsxjJ1i3ytKj75XLN7yxUSTDzR8eXox%2FPMkeSpeSJ877Azlill46itZbGv1Nu3N%2FeAWNWCUR4NbwLMxQj1aYYvR5rTZ2n9cQVWj3kMPf4mMgGOqUBtUiQUuvX0GD2GvkfH%2F0dKruOoQ%2FXJEarCAoCRnUKx7xhBvPCcQvZAznr7Ti7LWzXG9C8pecuums0Rv2m5mT0Swf5QaP2m3hA4SNii%2FG1ByWNT65FKBJ3QdxWOJddXn%2Fo65tnmx8cfBud4X%2FeoYh%2BVK0vMOe1g31rlpNnFMy72Z64ZTFJfkVsxAOywQSaGZkN2sOP34j5pEMipT1x6jy87XnTyRS5&X-Amz-Signature=a8925da53ebc1076013cf9e45f6ab1035adfb6c7eb228f91470232ed5d0ca167&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

