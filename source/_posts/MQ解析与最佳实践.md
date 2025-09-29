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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TGOOPBFB%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T190045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJGMEQCIHA6RYoaYBNwk2g1uyu9S9jrN9EI5JscyE7nd%2FPno5jzAiAYtoD0nUjbLa4ABoOgkTyNz415ytJmf66nT%2ByFDc4h1yqIBAjZ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM8IvKmjfSYVKRfyDhKtwDVObU%2BiFSlHGcpA6R4gzdn2bfDmuKlAf8HXMMppSGit1SnyUUeTILSnDInRq2w8y79gPNTts9cDVaH%2BCvxdne1kgWfXE1tKg8U6fHSomWvjGPc7XBqr1xnUFO2oigxGi3cdOzBA0hE0TkP1m8XmajL4USnMVVUCUAj2cNWPT%2FEkRDke4Ku3%2FtOtwDiwWRxvi8PisyL7RxSL6G9GjwpXS0Z%2FdDLPrKlW8t6r9ppFHCBwywe%2B5n95ijC1vckF7FEvxsRFXc5eTJ3pNl%2Bs1JsuWV4igHY0b3AKW9q7ePGaQNy%2F5PZqdzyzEiwMYcVKiu0bbd4bHzGeznHL1sZPSUGbFafvIgDmiiKDnpSbaEojvxkHb4vGNbPqbtQdHpj2zmmSOKpK2J5ZXa1iyn5%2FamX1Z4Zw3ZKsSQ7rA01ZDVr2lVnpfCfrZLz4uNdyHQ4iJMxShzvd8jSoQjL0I17NgI6fMiVrGxeiWWY4sVXjohwzWlCCyuMn4sJyPj2M5TFEOKr0Zs3zIsH2zE2%2Bcd%2FYrj%2BkuMF6uks3EfnnD7zC6nNxVzM2ewue10Dt3zkl58ZBE7PPuUcCZ8SpBNUob%2BzIEYHGo86zahM3cgiG44hPVoKU%2FwnU3FsZK5NooK7ppZzTQw0tTqxgY6pgGok1S4Oh%2F9Utod%2FC3Z%2FZFGNHxYYEyJ7XSTUO1jl5nRENNYRfrelWFE4cgsq1PCPYSrZlEbyZGi5FKBEIWK8PEMhjuhHbjiuukfbsMR%2B7563I%2BwhS8%2FYUnDLBrrzVxlJx2ZO6hckCIEsfsrKLwPldackKozlTYXomy%2BjW3IyFc92YpSySqSMZTzmPVafCWmn%2BmgQFaFE14GM8PNsCYriAHEmTWAtgAu&X-Amz-Signature=993f2a06931d06cdd7611a775e521b0e82c935f885f98e5e81ca092d89eae6ac&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

