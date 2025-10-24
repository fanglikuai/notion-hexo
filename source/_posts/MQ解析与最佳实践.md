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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46643UYHHO2%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T150047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCID6zB79FmM7bUoYLQ1UGVbK0kr7mPhMjnJXGaiVlwqV2AiBn1FV%2BmVDKEN0HCbOOUcNGrspC79qy4%2F8SAi1lsPCJoSr%2FAwhfEAAaDDYzNzQyMzE4MzgwNSIMc2kYs2EBz3xC%2FmPxKtwDVsX8tRw9QoAt7KaOhQNw0Z87SFqmGODRIQ8L4DyvkwPwkQNzlQclx8Kq5NWhrZua12d0KycwXGK6hFfNrPnGd4gqRKFHoGJZUIU6rkFp4PXnsEOBqx4LqmAWwlOULDxFqCzcyrg%2FVy9rr5dz5KAUlq5z%2BXgXNQQDL590qOIjyVGNbAA7AE4ZGXK8crEXQzhOXBquodac5PHaSmnchOOXHFyBPXaRlKy2ZUevc5ReDPKuW8Q%2F5Obq7bY7UESygFTZ4VTKSTrMH9yl0gGD%2BWnN2wL4F2Z0uydn9kRATCdOXOx9FfFXu6ukcA6LN96I2cs3rUtxgUvXFmAvQ%2F2aHKSLUo1n7n5PJjJ04Hf8zhJHxyRYnnRVPMep3tzFvy0KlfDRJSRnSb%2F1cwUgLu7DCu5jZ7B9m%2B%2Bsp0KTWk5mIt9uZaTeDomwX2k5RkiFYA3EuOL52oawTjU4l0938WAyTE81oSVRzh%2BN6%2Bzer%2BZ6tPq5XMfKxewM68A7qaNjcD%2FI%2FYs3yXHT2MwsGG%2B9I2O5agTfcAuVW5H8GX44fuoCn5HEs5apd9lBapKrGoEoMIIqRB6qKsUi3MwJwqbu9fGx%2FqeBIasz5MTmkZPihvyY0GcYRWdIrB9U2GWcKLGliugwxIruxwY6pgGK1f5F1fDIxtSxFE25fzGfgJQakoEAhGVV4GulpbRjP0YPuqhzlV7SNpjFLwjmbxwjjbwauZ9kGqu77LiyjP9yq5bXt6ndEmvh38NVMINirHpijcWCb5jc%2FRYua8NBefh06krARue4RB6QPIyroMb80VpI%2Fs%2Bq4iAPg0VvTA7TRSoPGvA8j2%2B3XqDytVtVIFlpKvaIfnXQtbDGZLDO%2BW0wguFxYPUc&X-Amz-Signature=3a6e09b18e833de7379a6e11fd0be68ffe55deda3317f5176ba305269163a857&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

