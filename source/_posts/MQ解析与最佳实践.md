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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TIM54VPV%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T170048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJIMEYCIQDL7q4QgCoZdZk2YzoRXuHlIsQwHqceZWO8BWGtx73hfwIhAL8rrYJljpEfckrSKIk34ft7E44PDXRsh1Lq5dmEqOcaKv8DCBYQABoMNjM3NDIzMTgzODA1IgyhFmwBWtNReIUzDJsq3AMZFeBwIQ9FRXtllUTd1pmuAE1BrqgwIN7O61vVomYTreEZFjRZV29M7HpeM6PBYvTi9Fs%2BWj3LMWJyCnYe1x3KKbo9mCbepTid2SXfUI1EVA6gSOe1nvGkQXSU9dHLZnV8ewezRw8U4CnRa%2FvuVvfnqO7lrtqRYenzh8y2sk76s92XstMxHdgzt78xhlun%2FbX5A25J7ZWrjORpCRAdIkegUMTtytQz7P8QwGQj0ee1pQG3ODnDLlNFWZpfu1FG2XqLX9Ea9xxHbcpRJBySzL%2Fu%2FU1fwJuoe%2FhEyL7GCbqaOp4alCzAHaYP5klREr5hapK%2FQ%2FAw3SlczOeOup53kGKtM3mH03uPr05sfMdMfO733miCjzL%2F%2Br05e3SeEOG6ClTO1KQVot5rWM2unCZCJ%2F6mrDGolwC4qKSihpAV2Lk8IK%2FQkFyN%2BDnhvbJG3zZ7%2B7tRJArqnuXXitQywY3xP%2BUJ7c2mYk494RmyemOh3y7aTto4JSu6G%2BSVE02Vhw5mKWF9I%2FF4PcRYNyqtn42bK9ov6uPpnouDbPO7jqzXcjmenrv6fBGGoSqGDI0h2XPA1wolLWyLh4Ei2foC6yU4pF%2B2E3yla0zx8MrM34OUIEzYXlhkpPLcs7y9%2FdFz8jCVpKnHBjqkAYrOsewUBGz%2BSX5LPe3BlUs1VIkndCAPnQpVhF7BLhqxeGeFhPNHXyMXS5DXP9LIevzE%2BeOEADBfrdzuLo2CEIgNCarrzk5Oikut%2FwxV0aq4GvNm5SsrtyiEXeobfRvI%2BpI0ER8O0d5Okb8vj4%2FeGnabhVhgXo8%2BMe0io7uxBdfkFzCEANmnlxLlLEFunptttcmFhhJZ9ID5KNHJud31VF1mfaEu&X-Amz-Signature=536cb1db8b5cb4b818ffe5e5160f70b799265d30a810505cb5b9904b398f6c0e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

