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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663X4RO5M7%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T160051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECAaCXVzLXdlc3QtMiJIMEYCIQCWTY0spzbyCjvTmHC5eXOXrpImZ%2FlQ9Sa9kbD%2FTnNmDAIhANtaj%2Fjr1%2FQG0zWSrfhwktgwCJQakKmSW5ocrbCmFLSnKogECKj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy6qDmfLIeFvFho1Vwq3ANhTlxuorgvrwjBJ6VQXH5xUx4wZLWiA8AN6AID%2FrCDrVGD76KYZs5vXMvMzuhtxsNqpLux%2Bf8pgs9Bvnx51KxwCqyQoWo6B2DV%2F%2BVXlvPxGmIgVkf1K2QImkwBuuDlRjxgL7037GFJ87ZVyOi1bgwzH%2Fj72k4omtT3kg2CA2Dd7FL65OIWp6hzuBlyNKS%2Bzuvgv4jGKODu%2BQ8rZPMfBSEtclABvn40oGv5oNzfyoSXOQKzeTM6XxKGQbHKxYy8%2F%2BJ%2Fy%2BXTwgfnpj48vDt9k15m4jrnxegOLC6I%2FvwXg0zB%2FXKuMeVQAlAOWP%2FA4Z6zuOeUS%2FvIAGuU48PbzIZbTgV9WgSF1EqJnZtGkpbKi974TOFqrsj16IW4FEi4u5SU2k%2FQcGbiZL%2BnjM8t3BT0T5CF4Cs57AHNT30xX%2Bu%2F9iTykCmPLJRndHyZxQvD0kJ2Rk9dtzEFH6COINsB8WNNlR5HQegSueDB4HDNJ3gpItxpuh%2BgZuvf3s6Z6pWAUD6QVjCd5DreiLmwarhZEvWeN9l28d8uN20zT%2Bwjrz5samRwVGSFFYghcrJDZMVzoAPyYvYJAa02ycrgdRcOLZQWfYHlFFFFYIm1X9INA%2FIkadlDIxX%2Fdm9cCCL1sApuvjDggeDGBjqkARIxhl2tsOxiMavUFVnOBipoVRJLrxY3kEc1V4PW%2Fq5vynwhkTc2yx9SHeIqQxdXZlOwyVKEk4Ls8734C0D9ke%2FqrHGo4M9Qe9qDYouKoWk7sMH1SToMvTP%2Fapzi2j8J0yhHqWVqorQd8sXNfzaKhSRqGWqUnDXpfWvpPFHaTLGHPuMRfmXYZIWeGXaAIiEEpWsYQnznZ0i6pEwDAsDMVhJRWcB4&X-Amz-Signature=86ed4c6fea539ac5d49dd9de0079accd98e98cc26951071d9482598601c96bc7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

