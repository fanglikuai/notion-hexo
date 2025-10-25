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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YLB5WPXX%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T200045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDWfzqVUO66VOOeXKaqvl%2FgN%2BpQceilbZyFHRbRG3IeKwIhAN22BomkSBze%2BSVQJGNyQ%2FefCW1Nxhho3V6mAqvoCx6MKv8DCHkQABoMNjM3NDIzMTgzODA1Igy9MVx7jDATHYsfquYq3AM0tdQn2xmgsNieSwHcT8SJyf1uaQCzm1iRmD0vok0mw%2FM6aUVsxKVw1RPpvYzFB8uuAdB4Klett8xvhNyqqihWlkna8dz3DpW60%2B7NvDFubC6fUZIMcCoC6xqjs2bmr6C2JhxPy9nXt3K4JbU6tuy14ufW0mq7Z9whCzNQ1XKdsm6LjkUsqFdW93r6rk3KEurFyuE8V6%2FMDYosZEwoBIGMCScWcxCodfaeBZHGjp4o5g%2BTXUuW9Acq%2BoKSCe7qtChH9hn2HtLOh0NPakFDCt6njQeP1ecdA4TZIvXnYjLJ%2BpCm58GhVfzroWb4ucv7yi9a%2FRVqMWv07YdEnugEO6dTgAQMxjoVDFtsaCZp6IUo2PzK8pKr%2F%2BMXslJLZwOhAkluHkyF1C02pnF5JDOCP%2Fktr1YrnEP4vg8tgwOUskGcRSDy0o8%2B52Mjuz3qR7JNyCiNw7LbmoV1w8jiUstxJkPU0F3f%2FCzrRhhz9FECamWHVTo1lFI4wmen4vmys%2BRZ7OmOpjRjqqqKeqbDp%2Frt2nVfLj7DCpSi1vJfWvcdEuINjVuGSKX1ACAI5KVufOMMNhZcEloSKnKni63dGrlYRCJFcpc%2FhSRnmylp4f00utCqIH0E47WOrqNTHQMB0jCt8vPHBjqkARzTSOa2qQMzPFWL0WQoqAvIVR%2F0vsTo473glfoOopA0h6fIerT5kWfAy%2BjVuS30B2aVJ4QC49i8UAL6uQtNwI6bRwAWpOaXFQkbSV2zsJXzI0p5WnL%2FUgHC23vTPGml1os977mtEb%2FYuFC8CP73LPCy8f%2Bm995YdNAUlji%2Fr1pJIx9pAt%2Fq61ZIci7dJkorTxZsZzRcfPLFmVWpcFw%2BnOPKmcTk&X-Amz-Signature=36122c67fb3a23038b19bec0ab3c8f535d0931aecaee2f4f12a8c545508d76fc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

