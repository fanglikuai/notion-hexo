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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VOMX4J4R%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T230047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCbRvNpjt46zA7sZo56a47CQkA2qi2vOXdiaO38hr%2BXIgIgZtiSLhB8p99oqNqj%2Bnx%2Fz5oUAG54Cw%2BHvRij00RdT4Qq%2FwMITxAAGgw2Mzc0MjMxODM4MDUiDJqLEBt9lgy8LnzIBSrcA%2FmcjTNIyyKlQhrFOSvCKqlc0%2F%2FkjJQUQ%2FPHUl9%2F3RKcI58YS5NzzFJ%2BOMuCaaR35r7NLJjT3ZDi949HTCy%2FuuBpDf%2BnvguxyipR7B8ycAChcO2JsKRg8A0fTorXgwx%2B%2BWgKYSzbcAvGmZEtacUrmJd7%2BKExS3VerPDu0ZYZraWcNNmjCO5%2BUz4cclVPcf0GE2SUpc92EZsWcYi2g0zX%2FD3W8JJVBvegFiMQEJHa0lMOAVE6XadPMJSW6IXX10QAVj8UtIpDtqpUKeuKR3sD2ryhj5MrLSvrTcIHc%2FwK2L%2BwCxBatmFf16gUFQA5bWmUsRZZ7448s3UQeoJp7fM3BpfwcwBzIARXum3hbRd8OsBmn7%2B4UhzHuVN7DMM%2BdV0MJ3GbY1ATz6P7%2Fg8oGd90yexal%2BHf5OAKsEFQ91y1laMA9Tb5DYeeS3WrcWRvKMdVEwAOPI6jx%2BGJDqYaAtskwRh2bB0qAN4pJunlBmPebI1wcE2nqpvjbdPZkJE392DX2grkfB8al3GmJB42LP8TA9CTAEh4fSc0vKvqRBl4EWD8JQG02ru3tLZ1eeLCbAMCNpX7Z9q%2BIYOer%2BskoEvqyoHztiJyYyA6muQkdPHFPQcQgdY2ezAzBfLHGlQyMNm5zMYGOqUBSglh7JV1%2BRlwIIgf3hABloVrBJGsDY8nWNDXfZgXdATFktzL7bFMQn9Ch4bNOiub%2BbTNiV%2Bagf1GfW1zTWrx8ziFz0Pa8BjdxmLO%2Fo0awrfNQzyPQXdvdEyI0n9DxXAZAfhGLJd8HS1Gtu01%2BlmXHTCDCzjqMoa4pH4UdxKZkJXjjytlwUfr2N341cA5Svv5vwp9N61jS3sFgYSIBViQmt5eEDKo&X-Amz-Signature=ceb2d9a93124b91a17b40333dab7fa25d94ec6d1b6fdd6ff9e077c5b8544d20a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

