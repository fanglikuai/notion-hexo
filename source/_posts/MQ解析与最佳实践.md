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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665DXI353X%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T070048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB8aCXVzLXdlc3QtMiJHMEUCIDOwldS7dI6soJEEVPLBsOOtK9RVzH79ENyQBhB4Z2%2BkAiEA7mWga0EDEnk3jf8ZZPjHY5hi9EiVFrO9wNyZ4HS0iUcqiAQIuP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDL2TS9mXyWQl9%2Fe6mircA%2Fn5ZRUHWq1D4zSurvtsRj4WZYzqrk%2FH67D9pC3R6mFBQzoLRCNdX47fKoGEoPJ5aznAByGKtG2zWBD1%2BsebJdMgNeSJ9vQhDDbh2m9xehlhWTdzmzqsx591TY3YVnFHXWWMBrxJOxNKYfklPHGFJ4LGRQzRvmx1axM3voaypFXHByl4RdZ%2BZk8u4y3whWOc4jwTi%2BnU4Y3W5%2F6wTmHMwuJtNMEXvHeW%2BUUmMCrMIGFk%2FmRhRIWpETBCRSokhekLgprKqFkUBV%2BDfWeLOXJMmftx%2BsEbmmKb%2BfswLNXNVgHf9v%2F1MRgbuqB8xBHnokYOgZB8WTiizAP0cbgVlF7vLp7eGTC2r6DxfbOif35bgQoWpdjgdVSxLcHmvshi%2FqU7hPzWQnV3EK%2BfccuuCM3uAGgTmFU5jTaLu9qyN%2FucnyQB19%2FSdnU4HxDJ%2F9fA58syA5nw3eTy5rg773z66ETQaDZyutYhe6G0BJM6TIferZN5OET1jKWpnbSoeRs3HR9janpqF4JlaXVa1LGcEvdr%2FJSdQ9k0vRQer%2FFulA3PVILDs%2FFf2ckOyLxSh1NVIKtoeVp7VceqxRtjLr2BhElFoP0FGqL%2FhC2pVS%2BGABV8DKhFLh3U37voh93z8rKJMKiNmMcGOqUBZyiAOU62%2FdvWJryKzcbTvMPHo%2Byink8JZNn2o%2F%2FKOZ90My7EfJtLtCOn926gk4Y08HRsR8wBh4wSXvuVlM09%2FXpGcwcWJv4eBzZUR8R%2B0h4m1hcBq8FrfW3qbEF2ew1Hhqqr9vnvE1e9gF9vbWhQLCWjswGbcO0lk%2Fu9KlIiQ0GwDH61aztS2rV3JYqFJpR1xeSIDdeCqt7%2BM5DFox%2BvrMigAJrW&X-Amz-Signature=50d03945634ccc662c600aa2b4ce13e5d9ccbb7de9f3cc8d6e660ae6a9ed2705&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

