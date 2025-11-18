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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663PKMEBNL%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T220039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAQaCXVzLXdlc3QtMiJGMEQCIAHINidy08HsrH6adFgVy3NoKcj4bWYwZzkveObrMflzAiAsPlm4b0I4R7MC2POmnZyYQDBbSqq2zkhJO7NjINEFISqIBAjN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMox1LfDFcyEWStdLLKtwD%2F5T976hi9ySlwwtQoGcCr6zIOQ2yFvtzYBeObgrXgTTrfMO24YsYdpIcUPzE%2BpDNkzYiZpWEogDiDFH20fa1ry5G2TGG%2F6v4hxnQ%2Bxdz%2B%2BCwt0baNgKtMLPnaS8QhyX%2BZJcc0ukXuGsLXksJFZ%2FzLB8yPFz47SjvDw%2Fo9LhsefA2J7N4Z62HJ49jY3Cvu21kU5cGOVVR%2BcvWIZ1LqfnvrcEJsadBXBp8TnXKkwHDSooqn%2BGJeifUc%2BaUQkDvSu2nKN0WLjGI14xDR8XfQjD39bxQf%2Bg0oohiUOP0IDI9e6Blt4VIvSw%2BZK471%2BFyoxGs4VzZB63mOqg%2BET7YkReuNPRVjb7VorFx7LGVY2utfX9e0ek0c8uK3cdzPCBAHzSWNrSp1yyx4oo9%2FhM2pMUZj7vgoIx%2Fn6UvrjmNYhNmXPSHuon9oP%2BPpko5DwLPPHz0eeEB%2BycKlut%2F%2B1D8W9MeR6jZ3JgGyoslrnasmdAQnQ2iGtkTNy%2B21TxsKHrI4rPt30j%2BxeRpnVYRdCpmIoYgnnJ%2F4%2BdG3iR%2BqDJNHaSuPFT%2FbiPiUsixd2IPf8l%2F5xrBNEnpLJbgZjfT3qp81SBk0Amale07gfuB5i5DMcYWri2sCjtTGm2y3QLBYFMw5KPzyAY6pgGG0t9QJBTEoIihjtrcvpqQY4maYN708MdjrfWc3xP%2FV3OD%2Fared9VdPNfBM7UYOazfDkUJyc%2B7fieq5TYE76bsV1zWn%2Bkhpzak1Yy8G3VGwPTgh8JcJalsKE%2BYNLFTFRae48rKuIWdmPFz9W2L8O59TMwC7bRwbuLRdG3jPsLKk1eWIdktRVmqlYr3AzTqyVBLAUEFbFk2LCJJcax3uYm%2FRe%2BoyOMw&X-Amz-Signature=e4f91896839d4cf1e96307d7a09e197513f06523a3392d143eb98871d5e282e9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

