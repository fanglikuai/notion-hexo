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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666UTCQKQE%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T060057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECEaCXVzLXdlc3QtMiJHMEUCIQClKUWkY6wx%2FmE6v7gy6%2B0zUBlRsH3Tnxl4dv6USKASaAIgQ9HyWyeLfSpf6MkLA5XxHcll4yFC7v6j8JnHXStTSuoqiAQIyv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBlFfC5dwzvvSCFF8CrcAzXFQ8F%2FMAlky0AUYOjUiuPPpL4shyyCtj875ux%2F6h6oFrJt91Z3ozc1hSNw7tj8y2%2B8Aev4Nm%2BMx2bqpF90CsdzCsq38yYeEVOPXL%2BxJY8zpzLqN1faWH2jULDlrK0ftoFVPmIUQ%2Br%2BMRHj6wBD7FTTpQfSdqsIdC9MgQNnL9Un%2BeiEQbUcJ27bzGCXr2y5%2FuUK5v%2Fx27tUD9pQTGtiN%2BU6ibgK8TsujqA237TQSBQ6jK%2B5ARLZAGgDfkR%2BzVVu1szhUHm5Fav2G3hvsggUlGsFBxNNcD%2BdNXOVHBPEbZjmcZnpzvHKGg32ti7IsVt4Z3YFk%2FsdyTf7zNw%2BUdLbWwBApoWpXqlsQ%2FDGY1EAq7qBD8SxeE7ON1UHVVFgM5LKj6%2Bds0aeAAeTs4L8jMwiXOkDG%2FMCuMIqqK%2FnstdK9GRkriXB79GUL5uMnXRZhlPmegNGIUtefz7kISreLExJYS%2BwUtN4BLKLVzHYbwjpNc%2Bw5Xwv%2BAkJAo%2FwZNeccA3c4KcjhBRdkuINiVr1zuYlXzARhKHv71ZBCQY2wlZ5%2BPeK9RNvlGD3hV2CSWA3DfgWTXq2JdwqS0VGkKULkjaetZjh4fzPc7kMTfR6uc8zQ%2BoCPrOs416IWS6WjLpxMNrt0McGOqUB1lovKZSWg6H9U5PqUY5HZV34pTA0ZwxCUUcHlfrFdWrIMyumZTO1dEHqfazrqZjU2YdjOiB%2BKT7STo7RV4dn19BTopOe5yN7YLDTEsBAizFBmNczsxqz7GqcB8D%2BvN6rXB%2FSPXAzgkqqH%2F454RyKUc7fb7%2BNyFRFK%2F7tufW0%2FooSzGQlLWjb5snV0TWq1ECUrvWJpopOw8wRUIAKNQUY%2BCrm0dKp&X-Amz-Signature=2229633c1d2a371292e0103aedd09f1f293ddd9883eb4c3cc9ff56c2687bf3d5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

