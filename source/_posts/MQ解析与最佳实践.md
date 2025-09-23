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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667GTODG5D%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T220037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCpVZgx2XxlX%2B08fHEhlk83medYXiVdQ%2FWQ2OTYjFtAzQIhAJ3K4wecIpkxi%2FAkTTosonoUPfYSg3jBQKhNXUG%2Fb%2FWdKv8DCE4QABoMNjM3NDIzMTgzODA1IgzGZ8fz8PFVJD9DyuEq3ANV1vvBRSbpx8nJTcwiCwiGTdu2ssYKIGY1ghOk5hOzhyDkG212mJd7PaHNMSnYTm9A9lcDejmXU5VPm3P5TVmOpKd%2Bs9kI%2B7cePyWjCgZBdaOdsQShJCVbNdsTWSpwmS3QE%2FR%2BGo16KOM1ychzMudj12jqAT1YGWwa%2FR%2Ftgxned7XIVpo5qeb84QXegmlnfu1lzZhIuNQrDkBcgoCQIwha3yWe6WKtUF8e%2BR3A33CKK%2BmC4cqNSWunEBM5fwtO%2F1elTeQaqWH1PnXpFenxitiFg7Spw1LfO7Mxnf%2BpGopoWwj6zGvh5%2F48SPCJUE6J%2BrmUfZ4Aq5C%2B9FrP4GdfcWuYXumS3BCkROOphOYj1ch%2FxNjT62fzDVDO89oxzrzU9GBEJTAGXktLWamrltmiUD8T9nD7nenxzOV74GzjHjVQC8Xx7gmVMlpfc1mt5iR53XsNYhEPuLfctEi%2FVkilvuMh%2BDIR2arjf1phthGYoUygW%2Bnbr4g4fWUAZybbcW%2Fs7WdNJuaQuxg8HOqCoQU3L5yZacpEIJw6iAhqSShgfdvAJP%2BAId4CBqfZXCCs2H7PsGHd0rrrDg%2FYX5SLotTU3ziOv%2Ff8Xv%2FAPnN40GM1u3Pg6qn5DMCh%2Bm6POjeM6TCNnszGBjqkAck5VSy3rFoju4l%2BMPqk6At42%2Fr%2BsHQkZRqyTWQcm0UYwvh21Wf9rN4Oi%2Bflftjbkg1fQY%2BDLKJrH7tk443%2FTH3y4HJ9mk6jrgmp6AEPxRYXYqyRrTDInZGp%2B396uQs%2FHMkcDtmIAvJex5qPMudmNzsodrDqBcvCRa6v4kqch6dXWOqoInF2%2BvzQ%2F1UQcupyZ%2Bl8BIkQkBqUzhsyEKFue9IbiWR9&X-Amz-Signature=4a8290cc781e5aae7f8fdc49c6f41e44a403b44e2825568c6a4e09a8e079d25a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

