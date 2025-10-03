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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663CYGS5MI%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T150050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCCRUFA76Zo7LAt%2FoOTHrj4UW0pkx6W85ZKb9xx7gKSLwIhAOOVIJDRKHeJdS6VqTu02P2lnXT4kkV1C5fcIrgGseyxKv8DCEcQABoMNjM3NDIzMTgzODA1IgxrGhlgiyoPSbmAraAq3AMgRtew3SCttCKgF%2FEPaOBYCxyhXl5axWqKhV4wt252%2BlckXqC3o%2BCL7D0YfpF%2BY2E7uS1o5e2FMVZf%2BhSMiiPBOWzj89TdG4BLF%2FeKj4mAsoHiaXjftBYYECk0uwEBkwVzxKOHFPMb9F0l3Qa6sHxBFQyPEeY7pc%2BMOhp1M3P2nlqdKrwmKjwghiYkYZefq14TpvmvsKUAhVaqGwprEoo7TVMbwHLtn5kDaqqasHDLmK91JGaCyqM1N7s7FdhKCn0jVho0Z51pdfInoFjBt8Za0YQvU9tMnrx%2Bmj9VYlTYwsWiYMHRtt379CyCNdHGKU%2BunyFBlEsmd7WmjfKUWRNAUz72rXtpv0LzaK1qGfQgBJ5IF1aodKC6tAB2u02ShdN7linxV3rjSCI9nBwHB9flpERviZMKd6Bb1vreNMbj6t9b37YwRSF8yQ3xX22TvRKohkbClP28gLkVlUCC1XaSLXupb1Owo%2BCZiqgWjYnDSrkh%2BaKO%2BTxFVjlD92%2F8%2F42wiGh3UuSffoFubidyb6Z3MtBkLiKUMjsUEbDtovh8IT%2BDx6lRWnnk%2BLJSnxqu3%2F2fMwmteC7AyG3plsa7EoRyKuGNaR%2F5vAhWmjR90cYvl7w5SLV7Bwcbg%2BlcXzC%2Bs%2F%2FGBjqkAfYFyQhoK14P%2FE2i%2FcHMXGK%2FyBxjEm0exoEbhLmOtnzghCrW3%2BioZB4I5z%2FP4IfstAVPJpJ6H5EnDHgRJWK2rKdQEKlSPwvYl2tI6FXJY%2BewEI4%2BM2Z9OUV26YP%2FxiADXClEQcQq5ssV8dDv%2FY5AgxR3tD6CooAs2zc4sM%2BWljGPArwckxnJPNg2ZNnMNNLJ%2BXS%2FmFoSqk3wCkyQKe%2FJsevkL7A3&X-Amz-Signature=d41c194682e5516a8e26a4ad82fcf0e01b228a53007b2956502a34fe6386c996&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

