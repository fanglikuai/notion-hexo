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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T55NRDDI%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T120053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAMaCXVzLXdlc3QtMiJHMEUCIQCuEqbPzlIyl%2F8N0eS5kH7A87owTKf7ZxyiLer2zaR9EgIgZLe5zpZ8iEV4hGPYON44VgkpRsP4QSaurwYrrEEt1ZkqiAQIvP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDA%2BBHV%2Fq7eBahkER2CrcAy9nLltRlb4uPmrEVR%2BOcEUk7UB0sJKXeQOWKR%2FFNe8BG5h0rxXfgtf7gUtjWq4Nv2HVxRKS3TKy%2FZYukdn2vzKcMmBAr5W5r0NUH2JdsddxopzRX3kkksoi2O0QQ17g8dqSwpfuTzwMy56AgRNr6QAeQBX5mlWpCOs29OVew9tQ1QWBaR%2BdegRbLmm0Eps1Ksy9yWfSRShdJfWpgGmdZeg5l%2FCt%2FFIHN%2B%2BPLH1zUi%2BPaBgcJqQ7FZE0sXd1wqzagXmA9o77dyOQGJJ%2BMegvnLm3A2JnJUfvAuJXUBuj%2FPAoNdsgjag1nm4aJaFy88iv7bdPEhIbCp1iAkr8A8f1HzBCX6%2FfYHH40jt2wD7hAmD9fv5KyP%2FEPR4Iqoa52xh%2B5HJ7Ag3%2BhEcA%2BFyAuJ%2BF7xBdhfSCOgV%2BfVhHHLiPypizRg3CXAajbGTGCvmZlYKmZje8ca%2BACo%2BNiRfQhpqn6YbDqdXYmqV2o41cvZhkroxU3uRpasAq46WoqWk2JsVCrm3cR3lSJZ0%2B0MgvdNt5Kja33cKbfJ6TelrkUS9t9RuX%2F%2BrcDOqxcYKkuZraqev3zBXW3VXLe%2FWrNdOQAoYgzZr7N1nMDS8GoicncdzoovUYoAWq5zdNi%2Bk%2BI6dMMMfIgsgGOqUBwTYL9Ay0kJP1J3n0%2F0aOXMtMftbjo9QmdFTbQNQB8QXDLHUo3sknDZIsUJNq4PjGAETgvCkolRroEIOFtFFLyCsTN9UjxygkDIlDWWJTaw0eKRaii1Wm5CHyE41QL7%2FfGdWP%2FtDztCgFJv4E52Oa2VkCeQk0popfQVZ2t5mWrY1h%2FqUvCIg%2F%2Bj0tRD%2BnVOoOwUeIx%2FvorkrGoH%2F3TFXL4Qpso77w&X-Amz-Signature=bf11779884b1fa122c98f5604cd76af73b08f2909cb7b4885f0a81446a766f63&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

