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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662GVASNLJ%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T070042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAcaCXVzLXdlc3QtMiJGMEQCIBCGp5%2Fh1yZgrG3791nGRIggONhS075DJj7st3Tuz36gAiBuhLJOcSaXgfcX9Tr9vFyDzict9HNXHq2%2FNSPKCulZ2yqIBAjQ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM6gXdJbZr1kjseWlYKtwDxQSAOh2ZuHJgRbEYCXiSZB6Pm%2B0S1AevMrBtpLJvAkEXkRexuFbAFAdmhT9e3KG083RmrvS%2Fy33M1FgODJwcvuY20fP9bcew2V8ckvlBAgwGdMLLyZriD01K805ZKIOhoPP%2BjJTiD0G2ceoQh0%2BndKy0qUNi7gLOv%2BJxS%2Fl8cQvbxtg9RzuUgVCw1AbcKgvKwcykEzDM6k48W6hfC%2F%2FVfJKbdI5MqElbFuaVXKi0Thj4Thm9JjJwx0VYOPL5jLZwalIvL8o9LQqSvYlul1DEVAZZS%2BpDNA0QbygHmOd0xZI3f8fWPC3H1fmrT7UOi5l7LDt7UGQzcnX8g2bYJK5Do6qpkjuZaGwXVufFK9oWqf75kULRVB25Pwhmj9qX3SViHs8ft1jDoNdKMQHnAVeWeL%2FjEGfafbfc3mDBQZpwyVVYXwneRRIU4GSPy0S0iYAJxvCko1AoasacyHmX6vQfOOCNFP%2BHNWA2zbQA5LAfDbUnuGJEuXn7LtLfDoBai%2FDWn8UIRzrvKGGpx90HuPFTc0M2NAWSH4r1%2FwunaPPKd4tkWLRpvidFdT8Z%2BQhWpGuRDP43eMUG3MOBjISf1uzJe9J1347YlN5Wa5vIDVBD14g%2FCBqIZwLNUz8mLJ8w8sy7yAY6pgF%2FNEuxEXm%2FtMcPxQP3KL1rSPZx0SkR04MyAOeXE40fKL95t3v89x7TTDslNmpiWDFLynuFq4kPd0NjRvpFEbiEnHB2%2FKeSXvoVXfO2Rg5C9DwoJcBkQzCXgBCjgyUE6JuP6%2FmPrWwBqpxb%2F%2FWtacYqY0YVMNOeJCOR41kug7X8F1y00YigbjH%2F8C5xw33eeGo9%2B%2B1PQ2afp3IpRm2zCyzbE3usmqic&X-Amz-Signature=8b33647cf80f3e4c55dd73e2d08148aa7999e43b56724a9a20f11877ff282879&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

