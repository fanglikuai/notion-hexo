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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y2GJBGM6%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T180041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDIaCXVzLXdlc3QtMiJHMEUCIQCwdZSUq5QsHSya9TPfCaOFofaL7fHr5bmPoE3D1qkaVwIgMLLVrJRyBUV1GPRcl6631NGiaTVITNRJp8kEw4xSa4EqiAQI%2B%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLf6A0STaMO8DCp7qircA2l%2FRTFnev5OrpoYuOfqzILwNS3j5pPPZhIhHgZKnH0yuPC9ue8kz%2FbjvpJf8PAmk%2FXfTOi08Vg2cWllvbRfHLi0rL4YQmGhXYQgJIcdewnvO%2F4R1kKfckF2%2BXvY90fXXLiHdBLfh24L0XPzSzvIWZEj7c8DaEB%2FKQX%2BfsCAV6rfAJjztDuxdVFPOS2fUE7u3nxlqPk1I4cX0xxr2zBPThbynXfsCZ2WW%2Fsy9eEfHWpSZTGtLJORAgYflNzYDcPCegNom7tX2xXbsRsCpJBCnJAkswkvUmO%2BGS48IAfy7n7HTw0EQxhS1NwasXvx%2Bo9tyC9KzU5k4JuwgnjowNEmQ0S%2F3khzRbybUl8PJbgZiNqvzM2NSm%2FGJLtASuhSyNrIehrwAraGwv16TVcvzg6Tc39DTl74VbQy1QmpBwKkI9O9itQwap2hWDxdbwmMIU%2FL5wvxhr9Zt0VvQ9EHqgHdOkwaH%2BZBuW1c6LZ9sJHJCS435SOeH1aHj7uoXP7Li9%2BCgmKhCNOyqEaMZm5fwXPVlR2P8NgC7UkauzWWsTtHKYbzhmjUwMN0qsX80Eg0fGMSgY%2BnBwGazQUAqlq2BHKjwCSFzH5WFVQlay9YIDU7dNAmNflxGRY7UAQ9ybmhMLSm%2FcgGOqUBuFRneosnDYfnusw9SCFchhXqHCLHiFOzsN7tbg0cFV%2FYd2dQAvH9o5r%2FK5x2dfTmFCoHRorXs6SLpvBHQi7hpSXAZsoZu45L9LpFyoC05r3My7htR1OQgkdIKIMaNFbeDw5cnmST%2F7YB9%2BC7fLleyHZUm0JTO84Q4BFnsibK%2FL5GUd6xNPRrJJOc9fleEoG67ZMdI4GeGXrkVuza2YxgGvx95t83&X-Amz-Signature=7ac7b64ae1346cdb056abe9caa42e1dbad30fbade08d65c55696ddedc61612a1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

