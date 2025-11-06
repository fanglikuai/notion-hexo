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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663EUMGBNQ%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T170041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEbBauEZfW2l%2F%2BT4b5tpzejmfv9iE07bEH%2BtSK8sVh0fAiEA0BPyAjqo%2FS6FTVGFC01yw2eGtjt3QI8cyd%2BxkfrbLPAqiAQIqv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCi90KLW3cTFtkeZZyrcA99h0u%2FmykVTzZtFR4XQLlG30N5VP8mpGNy92oi0rmiJnsTciCdTd4YQw50Wl5vTwoTPqawmF71gBK7lvQ%2FA91Vkd%2Bx9bWoT4OT1qnRBHV2p4fk7HU5WRXTNVvSa8dClYYpn3ud6TDAHc3L2jZyOcLle5TgPwzDzsCWzj79DMEFkU%2BjEXdn9oNiPgcUsGmKyVrgGJaDr15c3iGBpqww0iCVsZnOkP0Ludo5yf%2FjM8no%2BUe7VsaC%2FezwP%2FJ0G9zvIzM%2FpC%2FGNXQzae4Fuak4mKINDCqAknNpMGoravUA5DnDTRaVvh%2FE%2F09RMf1d01h0HUSRyqmsdwRC55p7AOrncbu6uPTXbrewsNsXT2eQCmD2y29IsYDQONUPvyDj2IVoZ2a0rSyC%2F8gOr%2BLqRu5b44rk4JJVXZUwNI5kUkntvTfA6bk4FcU8AhVrVDTXgwVp%2FJe00FsHgyv3I0uQDnGdDu1Tx3H3LCihytwzPmW3xxVaiZh%2FqFBf0n45Nclu8H38ewez703a1zBPqRG13zOFVRlWRdf71hNfWr9yZhFsxWAe1uQyJiNLzuB0ycQE1NgB30baGS0EbrNItLbqydk7mxezplXDOL%2FRRgFzC4OvJT9BUPNxDN42c5JwB8wbNMJugs8gGOqUBIJVZtbonBgg9uzfPvk78RxUxSa0EhtHRJ5NLe0Z6avNhOkbZRN8VQhN2P1qsgUmdyj%2B92qtLI3qtEhogrXFDRksELfCza9qFWWMwUFbm%2BbanDhd4jPJxRtI7Y5Qb%2FDQ51GnfB0awks0X6Rts%2FZtwYBnDmBn75HAhvVOd5b4AWRfv4XWAjMVK8ObBPwYaO9fJlPf3fxXgi8Cur2oTYI8ceOCQw1Kz&X-Amz-Signature=e15feadea067da7290ec4685fffd77618c585eef2c53471044604294a3a3cef0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

