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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667HQCTRFY%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T130046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF0aCXVzLXdlc3QtMiJGMEQCIBmTeE26sWxwMHlygtQ7Cpme1tTHnZfhlT6gm6vO3G4cAiAzHWZooD2%2BRFDsHDxM6KArqIPtXFSua9SBWLfJHGITOyr%2FAwgWEAAaDDYzNzQyMzE4MzgwNSIM%2B1JYgXo7Sdir8z7rKtwDXmzFIbR61hciv0X5of9FDC2QynqvSw3oHBblFvfcvGk4AfZP177aeJcbLPikxMb54xxBH97ToOQbeQP3w%2Bpv00ra2SKBnICt1qoGWCzuhaUMe8tlcmEpX8LtdJYXHt0pni6tOwhZ6ZHkPW0jRkCLNTJm16z6gyYbwJB7H2MGv6MFsr7GFIULo3FPyp21hlayfob%2Bz%2FDQjQXOwz3gYY2CR04JRxtxUc8uTdENkfzSX%2BYDtC67RfZt9Aym0c%2FUE3rU5iLC2%2FS6SXc%2Fz86lWK14cRCPJJbJszE83arAhcmEv7KYzgKnndSfs30PBq8GxwxivkoJ5n0q7CarruGAhaWC51FK%2FFaog9twNx0S0MJZYpdLyJCwbQq3jC76%2BQc56k5JJFXmFIMiePsFZxN%2BMqWof9KN8nJxqFpLH33JiauG8Od8EFv4nL1B%2BqbZ2tgPIMWqm57IT5URKLDQUlJVfnBl8sztHGtnc0I0UV4zAsQQyET6stRsnjAJrkltiTy%2BMLD555tqXNGd2fq9vsNVxRcAmIaD96lW6KHQGFenh%2BugTOc1dyMhK2VAMaICpS380yA0BY5ZNsJVzajXy6VeWLo9e%2BHiCXrzLRmk1urCysqc82qNZHv9r0cAW0CwtCEwt%2FjdxwY6pgG3gy5gtkEytqhe4WdWrow%2FdjYgePIKr%2BbER%2FIdQePftZ%2Bqu9ytF9bp1eoVV%2F%2Fke27IFFAtbISW5cDRVv%2Fode8XShyc3xSiNQVBg%2F%2Bfz4AorxtadpBsuG%2Bmoztcq7zCeL19FpUewnjXvdtfJn%2BQU4AyB%2FaezFJFnbWuLemWLFMhEMuXs6TYheyf214AyX8m1F0h8MkWYAft%2B8Xrk1wxu7gBM9vOmc4E&X-Amz-Signature=b93cbb21b8d17d7732a12d5810949259ecf2a5c2b8981402e22cdea2b7c64924&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

