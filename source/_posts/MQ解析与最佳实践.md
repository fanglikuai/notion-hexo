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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XFFBJRWE%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T220044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBYaCXVzLXdlc3QtMiJGMEQCID4Wk3E3KqnS94Jj2Kuk%2BS46%2BTevDlxWb612nlvq1opOAiBD7EoNpz61vjhkU2d02LOYX8vDDQiTpLwwQpsDBY5DDiqIBAiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMbejrq1rOAmlSA%2Bh4KtwD6gPH2RDgD75VqD2P3OvPgOfJsA0%2F8wqzoEbM94XMxqnEjXTfdGGZc2hoouSeTGmqTySTaF3cPklfonP4RXdPMrtNjXCNXJ8A6PX0OS8jDey9rxJb95ryWcEN6zE9q4GcwW9tSFf%2BycfHm9Lsma70JoMmb0POSd%2FJX59F6cq9lyONZaTO880KMndD%2FbKI1mOZtgtFDTgXMbGBxNNW7eyKlXBD88GSrEtz%2FDz4MmtQZu6QcsiCsj0rmkF86Xz2RlU4DH05w6QekyqTgcRMhnQdH10Ei8KA5nwKo9TjjQfAyUwUbGWOCPAnyFqpHSqPp7Y38cLiPuu18lAtEJtmARSbBWu%2Bq6tWLs%2BgPhFGiunLLtm9xOVZqQt04FLk4LMuepFI9ODBqh8FvhfekGvA4d%2Fw0mh5H7uc%2FDqJlg4o0jPF7qlyEH1UYuFHaBd%2BkaRkHX2xcC2375keqU%2FYs2fCyTHX88R1KMi5Z9FI6rysKXXFp3RSJUrtkLKLV%2Bajk%2B5pb%2F%2Fja9F37Sp3IHV1%2FLLzA4%2BPfiVNvti1aN9tM75Y3d0wVWPuSvWknbZRHX%2BFjPlNOzlDKkZ88%2Fi6JdWXF0%2BXx%2BRs1w3g7vD%2FlIaELsJe3GxzvTZVC%2B2MrY4ltd8eq3YwlI6WxwY6pgFIdvLTk76Nx6NgA%2Bq1whzOMMcGIOmtB%2FzXQdRKJ%2Fvqlzw%2BTjn1WdMNopNxSNHshjSFjUinoIOiQketo8%2BupibipSgQipMvxIKKks9mDgd%2BsSuBOsDc7F4qqfLwyrY5P1Q5NM%2BCM5HgwibueF6XSwYuawql1ytFA5iwtsPFAOkSM2sCr0k6VSw%2Bqs3wTbbs4ii%2F2ucMHd762xw799hy7xZ4B02LWgnh&X-Amz-Signature=5535cfd326e8483677516b22eb02b759471aa84ddc15e64343ad7af571ff80e4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

