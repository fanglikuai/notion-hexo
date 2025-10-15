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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XGDZ4W3M%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T130056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDO9rDUL4N1AhBbnbNikk3lL29ZS3p86UDxHgomhoOMuAiEAgLjvrl1q0J6npNsvSVXytj4uxT5RAusmJFSF9DumMLQq%2FwMIdRAAGgw2Mzc0MjMxODM4MDUiDF%2FGfhPYVprfvHwplircA98Ry%2F4Mur2Bhz2Ehro9heoht2urMltYexJ1H%2BS%2BWtFDShPybcCwCcmNyz248KZ%2B3ZTlyJ6pJ83XxY4YND6XvKPPKGidzb1mvH7u7xmTtKyU6Y2KcVsXsDtz5bGawNVZRfKm%2BkRfT9YPqbJYITH%2FkL4pXfkz%2FSvN1jxlehG77LxW9mjyWvXzwpemypSEycEJXOCFBss%2BKO%2B3cVimY64cRQ5SsvMnswECxgQcI%2FfCqDlIbAYhg8GDo0R4ezMrzD4kWq7KohTXQEEQQErICrzFl1T7fjYjZo09RU%2FfLEBSWwcaKfls0Yhw6fenFOj3irxytctH0%2BCdRxE26sP95Rr5dAMv7iW9Of6Yo7Cl0KltpOTIgoq99%2BF4Q3sRYSp25PyJgdlc%2BnZEMf1IySqu2%2FiOO1FLaKFXRJXVOuyn%2FZWLLR7pKllKFFO1q0i59XIEO%2Bv2ePO9cehPaWgQ9BEzeC3zmtolqiTiD%2FJ9MzGFGKYE%2FVe%2BGAMFerLmer4Ty7xRHp8eDuGNFiEPBgfyKRa1wd8zxYoZBkba1ypbTRBRvolhlcpU67stl%2Bweqb36gNtuoeEdxaoi1557BGhPCF99Ga1PJHNS2yHGqVi836M6PQvaoVlaC1M4DwA6D%2FBnkTVYMOiYvscGOqUBTjDfl7YPYIPZxwDuKDL%2BAZgUsHjqSeWMUNmDbwHQVTob9t9HpkIpkpzevjqScGQPr3s8qiZ%2FOfMszg8SMVSP6vH570hgQIizeIb%2B7nf9K5JyNFBE2dpT%2BZNEaPDdxC9eqgIA3e0ktWwruUupskmK0JdLJdC%2BJ%2FEaIZv5hQWAKyXbdCpJZNr1L91w8o%2BXfd9t9gqMy2kd49ohc3w4pmWQxSlo3pHb&X-Amz-Signature=e9d07afda7a0db8c973ba121965ed6cf283367907afbec583f02554a458b3548&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

