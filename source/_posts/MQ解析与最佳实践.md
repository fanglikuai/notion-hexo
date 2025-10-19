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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RP4MWKBY%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T230043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDMaCXVzLXdlc3QtMiJIMEYCIQDBNDRE9y%2Ff9mX%2BtDH7bBIYsgsh05jT%2BtRcd59IhmiC5gIhAMxlJXe5eDCXQ7omP2o8yik3EmDeF6LVc%2F%2Fm%2F8ygkS3sKogECNv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw0cqQB7s39DuYO%2Fewq3AN4Y6VWssViKrKHv2daAfo%2BHCQ8DwOuhWRj2ubsTFEVk0nhQjnQfKp24MydGwvhHxwV11RucNSKHfesMyt%2BRUR%2FFRARBjBqjVfCSVnmkYNK4FdStv1AwBFdC4ISuurPC2Y%2Bz2Ire2NEr5hgQh8NY1tJZP9X1Vvb8sH7oPHZcDVsvLYehBqtdAAGuOLItr1j%2BzVSJ7BCJ7fBg9V2apVzM5SKXhZz5lICClMwHlSWCtIY8TVLzkenpLP1CWwKSFIXeHT7%2Fk2n31dBB5Mpx4zkeYnSXvE5QyUZtx1snwW86ajsDpkyu%2F%2FdL2Zxx1f%2BeSL6sR6vyiF5UQwDoCicvblxENPE5KpYiGg1kil6kJ5iWxKN6SQ%2F12gHo%2F%2B%2Fp2Mjdhzd9GRTGQqJgQMwoZtnzrlu8hfxXJpoK2CBnNOeCtVV0KKSEKlDQvy%2F8Xya27FbRCNyRtFXst%2FXPaK%2FNcSwlIflD4uGrPnNb1yU2T4x7PKwbvyljQORdNwfIw19V5AW6Xlq3XKtf%2BrmfDyT4ihlUDkv5kdfWyQrAD%2FQE522V1IorkjHuqkNIdPj%2FOctNbtmO0Dc9lL1AnsxCpHSjZ27OAX%2Bz4pz%2BWzmGs89bFk9i4Qr89k4WpT5LGGM%2FEyaYzJJkTDK19THBjqkAZpb29n2zdYJJDAao%2FA1VVN6wikV6rN4RG5dJDnqOV7y45W2iB0WXgiiui0iuSCHLp2bOAKAwVAQIwRMBUF035CsN%2BpovJWYMJgZ26TEOyjxDuOWQUZLSz3YWIx%2FIotI%2FKjBLo3KTC%2F8SjYZGnP8R3q8lgfM0IeGlmbr9p9zpihxS0f14ZLp0OECQddb63Hm0d3eSWRxTTc%2FXpMJ3L3b6Q71JAoo&X-Amz-Signature=af6340d18bcbc16fb1f33e1b28afc90d84e0d15a3d134b3eb71198bcc2d69d03&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

