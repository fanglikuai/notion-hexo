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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666INJHI72%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T010041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFkclEuKJBSiF5MRbi3V0iGlllkkkYKaHHSKIq8aliUOAiEAvHrI%2BH5Q%2B7GVc4GQ6oZC6rA1Y%2BiVtRqGUNJY8l9chUAq%2FwMIehAAGgw2Mzc0MjMxODM4MDUiDA53PoH4DrUx0lgXoCrcAyWZq0YwMBOtw%2F%2FrHGAJm7uQnuG7hwn%2FHUV42zd1%2BeF6nakre0lPfI0NpNP12f9FoN8D02gmO%2BRfGxZLyyLkkrW5l3pmtXzczgVieAo42bO53O9sJI1S1uujUrFiKFmxrexlWRHvXmAH8vzKfs51JoGwyQzQHjKwxQyE4xLpWAovw7HvCnkSSR0VXHh8l1Ag7yLVisj3Tw9DfGBc4by%2BNuhDPbHUoK5uAVW%2BV065sSJh6WdjES6pN6bBRA7mnaXQNWDPSM8yv1ZxoZLLAAerUJxmk1z7IMAde4HliONU0TsRS59Avcm4k2n3w7MbMaxP%2Fbw12TDZbAne8FFyZOl5bNBmRIppb3A%2F0OEsea8P385GNSfDQqjtDwgbFosEm9opHyrA%2FIkSTRZyX1Dd%2FG7MlI%2BnGxP0k5wMbGHo1cZ6GCCfdDColCs0fUDVIaHT%2FGFVjay7fNmXCgrkyPU95pn1ICQzyfJyVcb5rN%2BVBVFY2rTJHfAsIDbcIEwka7ytLb6IxmLOm8QVxmmpKQ3C%2FR3G%2FQgnYWTdJKZMHOD5O60jqSc%2F%2FmADqByKgnodDegPYvD3k6NAc7nChyCdNdm2f5VbrWVHP8HT934bHzNsYAajlx5ao62nXWR%2Bak9ggez4MLiVmckGOqUB4zVptayZsRW1qWu67NbnLcpMUW7y0A%2Bb5O86q8lNigZtIsJp8ZNOvYkwkF1Aj6JSDzaw4vHIeQ9vm0usLE8wIxSFITfQjlL%2FlR4o3UN7WfoxFhSfN3o%2FCebSwJB8PyhfBt%2FcxGjcIq0IVAgXG%2Fip5b5Zwjw8LydVd%2F9S7dTUWYsY7ObbTjKRk8QZvBeIswmXio20ELrdls75jvjaSQxiwpnJ9UyA&X-Amz-Signature=b9186b08dfdac8e8283b096618fba2658b2b6a7709f8adb4299995fbcd644d3d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

