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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667L4ERB5S%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T140051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA0aCXVzLXdlc3QtMiJHMEUCIA9MgX5TbP9g3F5G%2BRD5MXob5lnfu%2BtC1YjkeS30uzNgAiEA7%2B67A3w0IFSAz0TuGAs3xxoN%2F0y9%2BenNddFsZOJeJDsqiAQIpv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGOFJQT1LEVsms9soSrcA07126UhFBNeMuXa6U05llK40ys%2FrqHPVjIis4OnFfJmMSyPhct3ld0%2B%2BZDDNXeZScad4sLXSYRFTIjvax1YZE7GvXeaSALsYQA%2FYmfFhNiW1su9Mqpt1%2BDyq89AqzTtLiESeB5ZXM9dzT27lgK4f6E9Cp7Qec6xDDzCfpT5HFrn8FccHfPIiQTXFYmjJyAWevnbNiv8MS8UhNZtC8aSOHGjt309czvHI%2Bn7YTk7AfjHkk12IHRENw34WAqz0OWG%2BxrnBfgEa1U99m7kPN7vqSnBO1CW%2BR67LR19ykeaIWlMbpFLMBcjUx1eP9ybZaqawLYIMBkLs0xLlvi0QHEOE7m2M1KPRvQhx71LtyUir7DBFduGeaEyHr588A8eTrP8ZVt2FfESmWecpAtoQjGQwf0g0Rlg4OgYeQfF6qpwLBpnpiDXKLFivRzgSRdE0h7vymEzL7uh%2FppTt2EjadObh43Ojaii3zfOVw%2F2fojdWrG7SCMSh5u6RPzDJ2Mjz71aNv44gmpQunl%2F7PdF6evDr6FLACyFFVi2N2avOZVEK1C1Kg6lqZcmn9UzN0kz1qgx13EeOKOXvEA9jocmC%2BTDDhO6f7uacxgfr1lcs3zEx54ku7Hc85ayEdYQfM0BMKOclMcGOqUBnHFSJpri07EQvCF5TsEp7pbvqu9R%2BWO7gUuWa2iv0jy9y9vuftZZPyaPV3ew1MLaX6X4K3Rvznix6623W8QTHhl0fdySoYcrLJaTILmLJiDOor0YrNqoc7eAJN5wvVNxewocT%2FjnT%2Bkful0yc%2F0Hwm0F7kWQGMIG90x3%2F3B0MzS5TudTYMMIB5Nrs4vd82%2BANKWfbubrXODlcItMIicLAsmZ8YHr&X-Amz-Signature=155fb13aee2ff9ce6687fdee54e3b5811deed4ac27758fd91cb9740ed2c2c2a5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

