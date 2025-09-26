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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YHFLQDKF%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T140052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAUaCXVzLXdlc3QtMiJGMEQCIDWDAGgJNz2xKi1SAlMqHpjdZPAq1%2BkF6OnnXP9EuEHhAiAsNO2NxsqB0B369crqr8u%2BAmqTxlQV4ZiobXN8gW91mSqIBAiO%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM82RPoEl9ep%2Fjvg5xKtwDBPDf53OhoDEh14vih8ufvUcue%2Bte46eReWBgh3%2FNylcXgGmSzjPiPYw1hVSZ3UMbopWwjyDedZdSABGWiKuvC0FnjR0mLs5WxxyB5kV82kCGSXvorsyOhC%2FIPoYMLL7cq%2BeKAxKjZZzKa%2B3NT%2FxBk82LhS1hCh%2FleI4GfmFv1UhCwAVgCMc2%2FOXdXRmzmRPYC06pcyVpWPhMgeFLdzmmWXKXZ7HM0%2B%2B4qbh%2FogzxVf%2FiDRcdyCAzMQYtu9XCc2OF7PBO4FsYPBU7hQjJ4bNs81A8i2ran3pMunX6L296XH5RmTY9An4Fve4mpJEN8OjSnDaPLp1jO7dE%2BlvUT9tZlOZvpGyHdEMpT4%2BnOYQdyfguuJ%2FgLpGYMZ8HCG5X4EueN11AMiqCedyIl2yERiQ9S5UlssxaIvtmpLOJURzK8ZLVDoiecWH%2FQmMQtlN1N%2BWxgvcZcCPV%2B9cbFxJQV%2BDjy6Pi1d1pzNnX1XHh2Y4Pp5fqYmCWtrY9vWXeEhgE3OldWJFqit7FtLhDWR6KG6c0X6OftLUcZhB9Tu8PxlU8VY4IiJpG7eOZnbUqQTdgNkB9LekM1%2FQa7AG0RdJOSo9Mn6de009KIGOPOX%2BNfsYi5CZluzQxd15urYR%2F3FQw%2Bp3axgY6pgEGjB6wcroe0uTXZeUg290rzeRZpiMrfyVlDZ6VN6TjSu2tlIVmrTwYJFVzEO2R1y3Fxc8OYB2%2F%2F5GKPQ0pqdfMDZog4NLljTk4n%2FAUbxm6u70FGcYNhMIRjsUoePksbto4vyOuxbzOSYSn7phOTRKr%2Ft5Meu%2Fu1b2NCqV%2F7Q7bcPl3m%2Bh7uLQH4rb1xI%2FGqYtLw%2B%2FZggEcXiSBMMq9lztHn8VnHGlm&X-Amz-Signature=40da9d806c1d825726fbcdfb37b5d718290cff7206762914e7289887b6fb9662&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

