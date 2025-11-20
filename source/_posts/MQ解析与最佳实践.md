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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664Z4O6DGS%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T130041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECwaCXVzLXdlc3QtMiJHMEUCIFQpleJ7nVFFm7JgkVHXvc%2BtLUJtB9xHT4crvHiydJ1UAiEAtFoBammZzzj94%2BOsrN0RYv2GfmD03Zk6o%2FnHl1TVs6UqiAQI9f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPcydLyCfsH5oZbpJSrcA23CPKW5WIsEWTK%2BYfmk6yg%2FWV4oG36bNWjJfLiAfGIMCnCpdu2bWGrBpRtoNgNpSEKczR%2Fj37EEUeSMWSnwI86Fus9Sm8yidG1%2BPojNaELfRHberjqP400F13crGvILFPeTQ6nTUn%2FZbcvgPWLB9vZvUKCaBiTjsk4TQ0Pff%2FyVEVJqcCFp%2BKxPohW0Rh8e7%2Fy5knsX88wcNpZkiMseKRaEtdHV%2FSYxTsPP6Kj0bEL1nMmTQiD%2F9pgVZ3JuRbJ90Zl5jfhDkuSEuwW9MOwKpSyKQpIeMCo9g6DY7D%2F2qldrC7skmULvXruvRr4tBtEnYeInP7g%2BstTfqRaole%2BsDqaiEKvICdtpG6xHmrDm0qZC0rOSpArmZ37OD%2FeS4%2Bzl%2BbU3llq%2FMwqdRo2Zur7Go34yYZPqNbBzx6h7ZUb12CN%2F1T9zz1wUneyKyxvdO7yAMAee%2ByVmAhMySDI97wbVOdLuq%2BgOX3QDpS4qzxDW3vTpEUa5K1tmrV1wza3VpnZQi1mE9Kkw5sdxMB7rVyYI5iFdi%2Fq5dfjxo6grnbgOEXZf7ma4c4Qug1D08kAKSmNnJiUXYCTLuVnXxojZZ61m5QEPnb%2BrXUANkBybMchSqj%2BxyeS%2Fewe1KXVxsDseMImK%2FMgGOqUBHqN8gG9aWSLwG0ugdBg1Rp6lRFoKEjQVirYlRN%2FjGjjgUqvPUwcdy%2BO51o5VVmFWylAbmyrznas8JxhNKk8VMr20oIar%2FpzHLQJixq3bZMGMEIqeUGK6rMFKoX81xKKnGTVo9Ub51At8ezkzgAShgexI%2FS70WFgS0ZhiymmNxNKcdGJ%2F0hZObhdo4%2BeVy8X4VA6GRl1ors8ayDLDAiCTVmvdGM03&X-Amz-Signature=9316139b7cacc046321b40166bc53e18b0e3511e74eb2504f164e6b4b667082a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

