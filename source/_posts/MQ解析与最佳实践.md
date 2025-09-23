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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QTBBYACL%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T200043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHnA%2BFx2QVU27oMUxqH69eSOYvV2qXLavRUaChMwhyvzAiEAz2WFyS9FJdAoUHbX%2F5CoEvOeyNCsWHMRo3j8sMMYwz4q%2FwMITBAAGgw2Mzc0MjMxODM4MDUiDAPPhIMKPKu2olcYCircAwvzFxWiDV4JlHkQuEXjID80mGrBvFqFX4%2Bt1HX2JixHi5%2FhYehfRp%2B2oNvQ0ZSSE8PEcMg5hgfu9k2HsGEzqvpGYCTrud5WtjOMnHjseRxLTjXxsEx3EsP%2FBPw3MIR%2Frm3qgDJ9XitRv3Xdr3LJQ13aRKFSY1NsdKUEjOZztFMo98QpgAG2xZHo4OJ%2F60vmmxD8aMamwKyjsUZXYs6V0bThd1redrONObwbYJBofhXxzTMkqBKYOI%2BWr0uxiEJU1hB9cJvIcicpwayN1rvbSjkzXKMfvjcO80XfHfWXH1abyoJk7nKuHrMUPfjUb8IoovMlbeGmEAPsUjlqawqrCR%2FzCrC%2BjUwSRglciGm2%2BRC%2FhUuhOk94AlE1SH%2BXc5CfCcGSiK6Oo0rDTFk7QDqn9NuHCD%2BK8lCme0Ezg5VMANDUF9rJkAqx%2BTvuPI2uO2cYyTmeQk5WAvYdsTvCOV9D38pfsep3hswvAh4vzXr6B1sYxom22GRDbzlCQedMRve5o4omZ2oIwYov5vDkqrp%2FErrv46DQp4%2FPXbdZaiukViOtYcs%2Fd6B1nNm0uy3u5Ok%2F6KmkT8%2B2nMIjs3YwcGqnqEuXvT1pu1UH35LUAdmHK%2FbDdiqsPJVSbmeaA41PMLviy8YGOqUBWURPzzBKZiKdN4vILEzePSU6ZU9%2FjKe3Sro1po576NOSNqUU5IR4Boy7iiun7wu6J%2FP5upDszx5lUW4s5hTtx4TDokTst1mjUYFcEYKbo2kMsCx1cENd7WxBsVAMq8JWywpuXMa6TW%2BPgW8pZNS3tZpNjUz%2FkTfoaJf3zhMwzr1QfCt%2FnxaXrR05bLUc6PW0F49gKNx2DWvxmCIpq43fyNfsyePs&X-Amz-Signature=c95a82620989620c4fd9a4cc69157990b68a8bf35f47392419af7d4d0baf40ae&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

