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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664STOKOJ3%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T110044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCIA9%2FJ3kybBg5c9BaHCggq1BoZRPtegiOyS6PTxNq6jgIhAOzUSVsOAmDTzVT84rE5NMQEa2cNS6XQFUEe%2BgwdpVbgKv8DCEMQABoMNjM3NDIzMTgzODA1IgxMYypFhylZ5waU%2Fb4q3AMHQ%2B4%2F%2FKRJqUrqWrySy8wPugg9kyyx5C0L%2BCFM9UNgj3RPM2TrqqN29Ge8Qe9lCGvM9caMzoJC4jKiyEFKPr2Hpq2f7ENCilh1pzu%2FrpgznJ7YRz3P0ZfWuf2ho5zu74oD1YYEnfDZGFut2NtiEjRn5iZvdc0sVd2qMP8jjr37ODTqJq4TyY2Jt6mcEnk4gcJAl%2B9norKwOBiFRXv4dKzrkZvx9aMD4E3%2BZDwnKvm1LtxZCF%2Foy3HuU%2FstIjCblFVfkBT7xw6DCTjmKAsUJpzHr94MqqwPQqiOJqeJ8cX0fdJyj7OfqRRKGRlrPdiaQvwDg36tuVMrZ4sXSDSwbXUBWyxa%2FUFQLSbvYGhQVs3voIo6ei1wqwuWl9BNwgTA%2F6HXRTTgqeXlnvQO7yvnSudfYZeFsq5s6iFI5YKo5yjiYy%2Bmbpx4KNDgLxrTO4IYnLzS6ZO6%2FZl8gIP5eNlZkmPR5%2BmBLXPDq3afb2SxqBSVF2nc8PdW9PyCL%2Bd5yKP0txXtA6C%2BVVnTE5yEPOi1FtttbH0tKv1GmIT7gJSqv08w9pjvgDvSJdyR%2BJ8T7t0X%2F0Vk7eZD%2FhDTdNaKUyHOkM8Efd0MvFSjqnSR1KYPRjUatJPQsjYAWHj7BQkUkzCX9ufHBjqkAVBOi0mnG9TsMcxjKfdqELUfU6OGLq%2BZmdD1L3%2B9SwPgZw01BHWlm4M9Go7c9h3iZN%2FvJ1fpK794DPnofiIukxx%2FKlUhVzMddqhCRAr%2F%2FMF7dshaupL9X6zGi%2BOvaK8Hv9ma5%2FI5Gf6%2Bo%2FTXuQ8Baj5TI8Lq7t2k6y%2FyqoHlsh8T0cpkGIQOGO9LyZdtzIL02DkbEnWTV3yWqKzgriRh%2BaUYAIns&X-Amz-Signature=951b91b0596984326604bdef6ed2d69d0fa50cb8a8cc478652f02c9340612b96&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

