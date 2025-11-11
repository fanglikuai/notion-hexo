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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663OP7IUXR%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T190041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJIMEYCIQDJq1vHvIduuIYhvZrkEeUm3FKYyueM4ZVFVqd8mmnlNQIhANFXjgytbinGKHM%2F1nRmbZTM9KWlS8dH0FKkuEJnA4HhKv8DCCQQABoMNjM3NDIzMTgzODA1Igytdhag5JXXWzgJsoYq3AOiRyDK6qJooE8kdqEBSfnC0JtPA0MoT%2FsLBoERjaXrldaH70oU61Ckq2hnSKINGORvaIiNt6kV5gh1LAlN2Q7%2FFa4WSR1ddnD0MtoChPAe9zljPkFORi9kvxPd7LpgZsrW4QvhrLPc6czGaBbPl8gIthBgnUNItr%2BQUN2mK22yYrLDUPjQbv1X3GVO37inxZ8OJli%2BKmyAs4gEvwAWsqo0OcrYdslIC1S3573UZkepjzA8F9EcuJVMII%2FsxX%2FA%2BfyhRHUVIIrlxNhhSI%2FJ%2Fx8F%2BU%2BMMaUWiUNhk6k4AtVbnBzYwyxo9vCaJkJcZ6o3bZVRxVo5prPGdQtTZPMvJKIe7u9P53K2uFWgcFZ8yn4KnH1UpKMnmmpYD1GrR9cKRkXkkChq9tVRtu8O0wa9zvECHQbnXSaO%2Fe8nusQ%2BfFxnEMWrEnoZEknwcInjL%2BTwb5rByt9NtNwD8a1dc0BxVFntf2ghq7jDKDazU3JS%2BFm%2B2bxR4dYHRkBQSozvHLl1GWvCM4zt9w5W5Cu1H0wQKlBIFdyRutwac%2BcAdRAvb8S3dQe1ysQLGmsPkkg3S%2B1OfVkqyfPKwvHX5Me6to1aflB%2BOvrcZh7ORJ09eUgdiPdxSFty%2B0J8q7w4CYWMSjD6%2F83IBjqkActI1%2BI0lNzfrA8hv6Kbb86UCwX%2FQngL34uMqr6rNmQzE8wYLTwWNtbenjWYWnPGNd8DxeuDYew3oZBFcebNYTGg8ozgUoB3adR794G7KtIQub5ZPDGRTbj5k5FWJRsHxTZ9OyVsVmoxg8ze%2FvIsKTva69wa9soid3jii0IYEKazVV0eY5BtplvLEqs4xCMI5B2YqKMGBXltsAOrlW9RHE5lRzkW&X-Amz-Signature=a9607bd7a9f8365360a23fb6535df6f78d0d88943ffc8ea1364c33957213d3ea&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

