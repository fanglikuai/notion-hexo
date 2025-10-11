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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QW62VZDF%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T030050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGMaCXVzLXdlc3QtMiJGMEQCIA9OJR88xrsHQ7%2FEPkqv5NigpoeJJ5yu0ikG7rJGAr2SAiBDPBmFVi0HyS6fJBtyM5VuOJiVkQMs7rlujnaRD4tspCqIBAj8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMJXqtklNOMCiLioLBKtwDkfx7BTMvJdHZZTYngSdMMfq7C65uB5IRtalbTsZpdCEoauySXbv8chj1ZaW8ckLJotf8qWfonP9Qk9Io%2Ft%2Ba%2BVcV1bdJDo9HjeXDmXwN0odIjR4mir1DWOAXn9D0JR%2B5d5MQ%2BTPfgVtuCLkl%2BjtTmfev7ZF1xjjeOrDQvs1OsONNpoHtoA%2Fbh42hj80uKiPbSRHY%2FPx%2BVN%2FFJJCcQzuEoNItNXK2W%2FyT8iAL89PgVA4dRotMOu%2FVmWLIOwqzRFZ09ig1K95T1XzmvVbY4LatKiVWGvCju%2BcZpiGnj6VTaf8fqCtRJdDZGoypAMlZd4O6A7mMEbVDObyklK5szPcCTdqJqjdiR7gxYGAo7KROoldYz3Kp9NxN2bAN5ssW3vl9op%2B27KR5yWbApUzw%2F6kQbyoGptskpxN%2BEIY%2BBGa%2FqNmqdD55Ejfrqb5OntNb0iKA2cTfy2b4Pbfik8qHCdYJIm%2FspBzmgHSvLfuE4jlKgVyb3Yeqft9VCAwIb9veQ760STe1N9DBZdFpXuSnq9m%2Bs2An5pjdp2bl5L0%2F2S5zLoW5XlmrlXx3Jj96bALRsBydh8xSdVDyKNvL%2F%2BcguZinmC%2BVuECHgQrP9%2BShifcpJOm04v4DTLrI8X4Z4Qgw94SnxwY6pgFKPMtoiISfZunNhTAIYFzci23W8puUsbkL1hxiaUdR%2BYPA4bxH2qhpnwPWKpKY%2BIUImhxj48q%2BhFcnjL8aEIz0ZoiOV9FvyDU5mT7BPS%2F4D89mGfm4LpFdNGMRrSB4sIcUOTiucq3GA99VV%2FY%2FJbGfzLmKewxFkqnklLg475QVUvph7JVK44JFKrO2vvOlRDsBD5V1kYYfissfwerY8Bqvl2DQ2f3d&X-Amz-Signature=614da3de65095c77c1d823308e67ad3da8dd1dfd904b45ef03b5c5bd262a636d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

