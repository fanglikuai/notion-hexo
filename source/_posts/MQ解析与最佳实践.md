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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665A7VJP5I%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T060041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCN3QuZ4SEx5DkjGWMW5Nmc%2FW3yEqQRJQCwS%2FmSCRngGgIhAJ%2B93WnGxD%2BoIDxI0OdAnwiqlhyYM6YS31ilDtBqpIwKKogECIb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igx6UzHNovvTzsWNB%2FEq3AOt6Ro9tdYvceU02oO1wBNK59kF7JjQN49obBA83ZI4WgrIiKQoVBUMm70Vi86uULpYhe5O%2FG98M8mZFH1xptDL%2F2SmYvxDxBzMixQ2Irl3036CIwzVMK3LicW5H9iTj4GvNi3EM6nTylikPwpEefjm1qMxtbY%2FwPx5W8NusUuObwioXckgWhoNteWhtFA5P4PvSrhYa%2F3yYaIVQDq5J3i%2BFDFs5z3YSG7j%2By5nIg5CsJsAccFytCXNGT2fTkyR8BRau0q7aryOTeocEs7Cebf0PJ2OJ6ttdaeB9I1YyaPun3RmaWEaLGLajDlnyGxyA14c9qKaFaRCxWeCiq55chRxRgysKl7Y2yS%2F5KdMHeZTuv7xqcFWzUg20rT%2F2FJPQ04P7n8xR9ylvL4Y36mri1H7ZINoHGCzpiBps%2BVf8ijWpA0%2FlC%2Bmp9OkN11uZ7P8lnREgYPP22E7r8HgBKAsDUQYW4ZPF82o16yo8qwmBj3Hz5GbPzQ0ZWDToW5EtZdX6plRZFW%2FxcAbGFjoHbUb%2BIDNGcPgcw%2B2xKRgtMKXxz0CIEb2mPS%2F8LlEtuJiNYP1Ecr2L2%2BdRwqrwzCX4RFeMtOXBAyh4cc7bO9yoFDZOL42HqjpHSrRkcfgnKWfvTC5u9jGBjqkATXrId9M4SHAX53%2F0VRTAnWrDfpzlyyw5x2GKW9pfbh4MK%2BEoogkjdajKSqphsiWDBzFUiJS5Kcc7mkF00P%2FvTcx8hTW4D2SkfWJYpDOwP76gNSJbGwDPXjM3DNtpqgT03OQh0QRb%2FyKrn%2BNxIlY2NjgMaw2C4LoEHe8XXP0blKrkzbaP4v%2BFfOnKVPGFmp3E0cDmmb35KcAljK26oPx4q6sW3vy&X-Amz-Signature=53460a677f4d2c8afccaf522c2613f4657ffc8c89943879ce6f605e805a7fcd3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

