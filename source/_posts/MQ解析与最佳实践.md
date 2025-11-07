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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SZG6SUGT%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T000041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEkU5FXKLRKGspDOeCft%2BpVlhfdcj2%2FFq7XJ9b0AK9r2AiBA6dJPo9%2Bk8OwiT3fJQRtSI7DILCFu0Y2bSShrdEAHKyqIBAix%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM6UW3KakfPTX5fpwRKtwD6AlCe5k%2FW4efehCgNYoFuOd7ljIt1lRszFmpDmub1KOxlCQxmkgt%2BonJV1p4zY0TZidG4oTIzhdO6kozwGNh95xlc1jCxDw%2FYbi5k9ov3zXDB2iyTOAiE2reIEL3IkzlMAq6QBMv380u3WPjqZtl%2BDELByhBnPuKmiTfSCNduFi23uQ4K%2BGePF3eh0sQkusLGGeA%2B3HeFbEKVdsR8d7CiZzhRvR3etrK965ZZqvl1Z5qddyCRn%2FMCCT4lV1cHq3Z7tcmSd16JT0IsgNTkPRkdIq7yI9S8amqecl1Au2P%2BCq3ZreCzjzRx7wm4OWpomtpRLchvyHQRLcb3j7EI0D54%2B424Si6OFeqGv4%2Fag6iQtsuGGdUuNg%2FdIQCwvuVA06RvrKemv%2BY4Es3fHLYrR%2B9N5iJHFKl310L5PKXNcWD%2FZm0SB53Zmv4UuY0XVlj%2FG058ZtItDmJ%2Fs0U%2BnIH8OKwPc0sTQnh97jcok2syI8fx8bic746Mc0j%2BYidjEJZ7GL7s9356QLhFg9IXfi5U%2FqGAgtINCKx8DsjbWTvXAjM%2B%2FGoTfebPPHjVm2kgwHfl%2Fclbd8PZnreC6%2BAMTDpxawm3CyvpqnCHpPaVT4oWBAGW34TZky5%2B3K9Ui996gUw3eC0yAY6pgGz86G%2BCEtLHhtWqdJoFllbtKRejZ4pygHTr%2FtehVpuikEKalTgJGCLOuLew4CCaPE0vyhByYNTvDlPEud%2BGDXJTkTJfjT3345uZq3pCSvG3Ea7GHi8orP75CGxIAwjwMyjOCQMrGt9SJbKqZ4ZkUAxnRXEkwTW1Z0VJ29gFWbQbeOXGVNbQIxia%2B6F9RtEc7Na5P5%2FDLSBtnekpoFJGUVNMcPwnIoU&X-Amz-Signature=a9c14654c2b397418889a6ac6fb378930e53e0632caf81c2ac72cb914f798cec&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

