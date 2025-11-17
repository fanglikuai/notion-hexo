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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667353DXDL%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T220038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCVH2pmgatYQKA8Ts6EGMaY8XrvbSpz3NPIBTTaHt%2BLaAIhAMbyCKeKcU8vKLQKYZgUZazoNyeGEUK46Rp9Hg366ulUKogECLb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzpgE5osWrXXvwloPUq3AO6r9%2Bq1OCYYLT3TWxj5GSjxDfonCDHZGKb3hyvOE7hfcaFITD6vlqgBenC7BuvOQ0ziJ8RhI%2BdQb0z%2BBf%2FFUb8aUnPI%2Fd57jchzWG3kmMyD4%2B0pORmkXlZuS9ok3ymioYiRRc1luYZm7CAqfFtSEDL6nDdeSkejC%2B%2BPefGaUCBIsrE4%2BUZzzdgp4W3p4D9g33by4XRJ4QbUn6%2BplQaVGGIrMjcs07E218tvdM4eiVUT5Pd6sVOnv2xGSHf7fpM20ylrJUB%2FlGj%2Ft6Vvu7DQ6jnHy1ZI6ShAZ%2F%2FTB0qAzSozj5bl9Xn%2B6XSz%2B0eiXqcPc4q7H9TnVcgKzPAWKbtXWBbMUv5Zw2tjgf1g8sqFWS5%2BS%2B2frruLApBHLxsKYXkB4IXxD%2BMTDOov3nQibw1IyIjLUE4lwsz9BMwVJvqoKAKy4C%2BbyeluCL02zRCwf3xQphq2tlKpyNHoCKEFEPqI8n09GWT123xbKORpsePQqc4zrBFGvkI7Fkk%2BZdiA3e4ZOmMMkvuW7rWkIwv3CYCC1R66YdzXnYtivSs1m6khS7ulJuW4kPLLWLai87SGqKzXQEwfSNJAOnkam7jId1fzlINlxU8IPFYF7PI3gUXMBjUtd3GsYrlkugv9h9mwjDJnu7IBjqkAep1t%2FNeoNsETp6xTNWA5KMqvZaYmf5uAungh70VNmEON%2FLP14Xtkz0NzuvZKifX9sk6kFqUkoaki%2FcyB4YUjT86ki3akCI8QaUDdJ1fWN6Ax6uCRuJhq4b%2BJke0rpa6Nadl%2FWLTf%2FTnmlD40GYbXWMgCEx94M1sqV2Zokqg4sEpKYu6efJU%2FYxxnUeO9qNeW%2B98vroT0CQnktOF%2FUZnaXs9hW4F&X-Amz-Signature=dc9dc3bccd7c7a8543d3cca3c542568e47f968b1dda826d8540b13fdeb1d66c2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

