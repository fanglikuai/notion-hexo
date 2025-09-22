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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YW7BXXP4%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T120040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC0xXVN3YYG2ibkJW9bwo3pURfh7zPJmPn1SSI9gFkhiQIhAOpX2PbK%2B0h8%2BfcBEnZAaIVywzyhBMq8K49Ty7jTPu9fKv8DCC0QABoMNjM3NDIzMTgzODA1IgyQmUr7zNKC%2FLp8rqsq3AMKLpvAdlPIVZpiD40jBZg4bXMncpsbjTaaCnPPox7O%2FmzGEruYcGG8y57Xm1W5eeCFlSOhfclfWYsyM6GDqvLO1RkYihKBH7jykVzhcAT1GzOZULttNJxic%2FXu9vWFhWKOtnJTMusR3aRnVID%2FluUuma%2Fo8R4kbfdhOxkfjdpD%2BmwtVpbr3rtyq84FawBCkJkcXrscS07b5SvJ%2BYAfn%2FU3LjjRH8QTXMnz5%2B1ck9oPRpn8TEdWF7rPg96VnT2thQ1n9zxTff3lMr0DIYtsTHJ4KZX9dUydHNG%2BvxRI7z%2FFEo51IVTyUt4hS5YDvUKG6%2BFGn7TjSlB6Oj5jScIP%2FInA3wrJswTP7hVtPJKnkJWl8Ifw%2BTX1U2p0zOmbO553K6gpYmQMWVp49O%2FUoPGs3OJUAjLhGbRUBoB79Nsyt24CyTWIdXvBEdVLmtf5LBHkJBbsNAV%2BAAyxadJ4AOBDH8QImg0evlXSAaJoE5IPnunrlnwgoUhIWUCe%2BrPWB58yzNgth54wkfb11fKJMccnhoduJl2sxLVuhj9CUYKDlhfz6mGh0M7hbMjrV5I3SatKo0uSaQmeSV%2BvYQ3lwb9HsSqS2QL6FJjXV%2FL66RpErXeM45S9YQVbMX6%2FXN8KXDCo7sTGBjqkAWnR5RU5lIfxd8JKCeiBDSO58WitAdV2oZ%2BTfnSGUs5YvNkZeyo%2Bd7XIPLENoUap9W3dTbggST4xhuKRYpAJjBUN8VHB63JOsfUEkIwe4MAdD3QIgYnuiWqJLgp8FteEkFz8PLV4z43FOP7%2FcrvBK1otNdtiw3TEblup4%2FEb0YERKbkY9rpG8i8Luh2pKHQi7CA7JCtsuZmivm%2Fu38wIQ7w1jK4l&X-Amz-Signature=fe328f64734bc7ac5404fa5fc4e0a8fa429d06d1bc79895511646d8b8e2dca19&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

