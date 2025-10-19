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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RLFUCF7D%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T200044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDMaCXVzLXdlc3QtMiJHMEUCIArjIJBXgNl6IsUmBHkLaEuZJquVuhvF9uIWiYkldi5jAiEAvn4Ecpbg%2F99YWLruOOHr0%2BCeHhpMdzksRpA4U47ALWIqiAQI2%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFWn2e%2FDk6UIwZXhWircA%2FjB4bH3GgzwClnGb%2Ff8J3LyDL04n%2BB%2FqebR5YN5NEhQlDD4Wscb9Fvy%2Ft9LuKjpmK9PPtoGgEwThhgBp2JGM0IuJoM7JSrcBXsDqsqWKkAPGoi%2F33beKJGRPQPL2Kz7eRpCXukB9hH3lfbH0ZxTmJqL1CrJOwbAwXJLkdndSNPWr6asd8a0RK2jOrctus2Nd1%2BaAeiwTh33v2OBLPNnDtMBby7r2uxNHpI5Jv9dLMJfOSiPcTBjN9Pvv2h0twqdvWCJ9mRXzENUm64ExfAULyDUYl0hOvrZ%2BLsnNBmfn2ZR2f0EcDfqZvpDrYUFuR5vnGHo3II50%2BqBCzi3551ty3e07ex0SucKdA2YOAckxPftFQSoABBzbSQaslEx9MFcjVbawYtWVuLq39FGVG2gsd2cb9yvlneFIx1ESrURSwmsLIBFtxSUD5JF04oO3rduk0G0jWlnP2Wv5Ow5jn9xaVjjrp1oIhcCNEhidSDgZCLjCz0WkQ6WlZIKpnTehPk31u8ED6Hn73%2Fp%2B61N%2FRJyk3IWCI7uDw8HkwfwvBRqdhw0M74I6aCVDHMooSJoTh4pGrvoT5JEPib57tLzRHoGqo0XjmAsAEBisDlGNpn27QWOzEmuMuFhE%2F5CBokOMIHX1McGOqUBs8s3OPXQ8e3nDmhcOcOD3vdr6xJ6Ch%2F3AwqSuiKvMNNlb6dp7VhqzOc8SyRsdXa7exR5gHQrFRQy2HSCONVnbn2XosiUOPJD9v6JH4IsQ3KZYE90%2B6e7aTuPuX8rkZUBscC%2FzbuS7OYodByLexc1XogDCNWZfBywBmEqAPBTxm4WxQOCBvKKeoAJITV6IfVq7DIUSuviGgIlHx9TCMVZzMVNxxfQ&X-Amz-Signature=09f34a26679f54a67082602ea2d0e9ce46b0f658388131d23aed734b56d2c2c1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

