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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QHWJLMVI%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T060049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIG%2BBSz0ol7MGc8B9cWS2MuKBFd3WSxfmnfMRWL5%2F7UU8AiANYoJDNyaKM5ociOr5tHFjfm4GkL8GhE%2BvYXyhu1P85CqIBAi2%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMObCQNHiI6Ms%2Brt5SKtwDyv80eTkhXkrtPQZocEjHPRvc1Zo8ATbPUL%2F2ILC14OhFi%2B23tGx1%2Bv9XmhzJhmFqMuwBiaXhqspcMlKWqiKZDpb9zMiRPktstjg4AIVaxUxuH0knW6jNYuZ63Ho7qQnOIpT%2FbHrODszHik8lYRAUWR%2BqxezJiGP9g%2FRf%2F4R3ml9jIBr7FmDaQb20ZTKnlCNN8Z9gFQSqlz7nGxVdOdCRaweEpMX0pewwTEJCYpnTA%2BPXDFv7j5Lsc%2FHUy2L9JlLQrpoJQqE5L9vJMRC7%2BplfrbyEEYqCBUttRu%2FZ7vprC083foE5anFdwiFhD%2B1hFliXlyA3krLaOET2nq2xL1a9scaPo9N8zvInzzA9RjeLqX5HcX80kp2oatOXMvlCHUSxZQ4q7QtTuxBArr15FCoMQ8VGyVHDXYbaIq6Ig70VcVmeW6DytTRenyUUPSCpBHzc%2BCMR19hiWSMcRyZdtkQht78aQh4nGlUEDKODzn9YsaOWumZwYC1i%2FgJnrGHJhdwRf329SX7tLxDF9V2siaUEKolLPM3STiEN7phTU0pBJGLwZmGc96Z1j5cUbT70f543jypeV712PlWKFo7Hhhei6Cyui2AQFHbt59c8fexTHGiNrU3sGpYwy1YFZmww4Pq1yAY6pgHJbsCUa4QUMdJSQpgNMmesCvhfPLpURqrB%2Fc7t5mmbXiSOentFsZiDfu2mh69aMiPtZI7B%2FDZSvyVqNCkDVTPYOXb3yvNcDMDlBVslgvQnKy%2B9uNQg2SFgjQQDORTs2jps94ECQCZdxHy6XCGmbUtU88DaTe80KYqvZ3VcAYmeDSqOdZLeXLW%2BxozvytuSuLp0CBxa8AALaT6OXLTdF9TcdALzQAIV&X-Amz-Signature=49666181000b10fba45dce7fa792fdcafaf56ff2ed8c7b4663fe90e050ee29ec&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

