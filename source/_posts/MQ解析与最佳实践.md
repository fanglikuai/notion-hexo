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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666ZAOOVG2%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T000047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED8aCXVzLXdlc3QtMiJIMEYCIQCG5lLmMBIVzdDLFEiV1VTXTGt87%2B9knWgUYcHhOGeUnAIhAJepOpvUBudsZ5ygf%2FLyupGYSCBa8cD3BXUPlzncze%2B2KogECMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw6l0aQn7rSXx43fJMq3AMoHlFe0Bw7bVdsnBIvBamCkbMJo3oKMFRuZj1sa1lPok99zWox2Kk7U0zgRmc6U27mLWRFxvbWLyjBAmE8opFQhXpHeUSFXtUrdJj0XHvEHDr28XHI4a0SxRArmwTslOeYT%2F6f%2BVZhv%2BDfYGDxeXhfy1DuZ5Z6gdkUau4jpdzQ8Mrb2GOGQNY%2F6e0vXCWLIH%2Fvsort1mNFPJ2O71z9JMN9918Cy7hFU%2FAlUY09SPavI9jpvCV3PG8ZgHx2HiPLRJUWGLWv1NNho58kTNzPWj9wMLxQKFG6W%2FzLaXCzA%2F1AwtNBw9AHZU98oueMyUHT7oGDW4Nce7LnQbGJRxMDtSqMvVWLBf1oc7tLzWUtY3ajBg4fir5EfgMzM3Uyy538fX1OkyyO9D1wSgYLjvbP0TnDCWjsfwdnHwjSQo7TheVhnqNchKRGTUGxRss7wROZjRZybkQgDe9oyoN65Ym7OuRYtycQLcdfVnvageoQK%2BMw%2BHM%2F5%2FDejdnz9svllxtSb2wG1eU4LySJx8sGag326LfPRg1N3gdvVeVNZxUraZa4SqIOTmSVeOYSlwfH9XoKhykiEXdW20VZPqhSFxuoTfXXXGqK6mZZAOV8QUPHiP%2BSZWk5Lk%2FKzyUxGAY%2FRTChgufGBjqkAcAjxtT9JrTZMZwSWzL7vcrsKEru5YWe9UmNSNrayje3CzgcwaarVHYaJWQxUiP4PRuIo4UGUVf1W%2Fy1pE7JTO1ruLFva0hROik7IOzJcMBYot8PdtUEqPXdCY77Vqo85vFwEhhBwY3PmC0gSokf187s8SCfwzw4YDUcUroc%2Bkd%2BxF%2Be5ndafg%2FhQKd1NATnWo8GdpPjhez9ALehGMEUyTZO7tn1&X-Amz-Signature=4b9c106c6e8e9dbead64bfbe5a20974bcd4d2b3339eb440ffd47d42e4d884ed1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

