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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XTF2UMWL%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T130055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD0Ct9FZhuqfaBHkXADeJ5k1t%2F%2FdTNAm65GuTWD8yxJnAIhALNjF84O436N%2BTz1hEHfCqZTi4YapY8Uel3OwGM134UMKv8DCEUQABoMNjM3NDIzMTgzODA1IgwR%2FWDAqVP9LFiJrYAq3AMGUpppCeJynkaapZvyLN8DAb3%2FPPWPgXlNCEtygvLY47bZSHI%2FWt1iEkvxl4N9CLwTDaNCgRYLr3BekhXk%2BZiOWqmn2nfiz0WhhCOi4BcrWMrVhxH0fT9zqVeBe6AR8hUh6CXq1%2F%2BJ%2F8ZjMOa68WdKJ8n6aZMqJH3ybEhAZJ8%2F%2B%2BjpIZmdhyZg2MpOyiFBhtaG%2FYw%2BjNcEM6dWZfg78SK%2BzPDATCldManfVLzVo8ciPZLbEtZ5f%2Fnb4wrdSeUmbnMSWvfDrq11JctipSyg%2BB9NB7XQ%2BtzszD2jIEPNi2AipfHrNfgeuWjnq0a5Tn3IdLIfiVVwjMLa19znI1RN0s4YJGM6G7QIEvmUb2z%2Be75mq0t8LTZvXkQevMhCPeDaSy2085bARtGYjt00%2Fa82fP09dgeurP3TDaEk0r%2BVX34EwvDnVx%2B7QPZmfSGf8kKT9bHaYI1ffLLG7LSi3HYs21DYpB84QJ9MofrX3LIStvBUd6SCQ9FEMKe0bchU2jrhWJONFn7%2Fb5WzDFcAzdWCnlxlVebAtdPBhWZWxl%2FnOpcLMtVgypQYJzvROmJFpMtMLr7uViDp9bWtH1aTqnDYVHPH6zO%2F4nlRnDv8sBO8TIslzrQXcg%2F8nFbJnfqmPTD08P7GBjqkAY4ESHnwtZ0cok9jEwE41IQmTpuVGcMFQCkRXPS8XAiN7hnHQmyxXsY1g5nurv%2B%2BteRMtMvyPN%2BCr219EGknAKy%2FbLXco2RFNx1LLiBYnh02IWctPqq35R4Lh59yH9qV3u5%2B9N8yMJYqZQrFonEou8TElgevQMpz4N40GT1e9HOymazCOp3HVjucLubEiuAcdoLo5DZtOlkXtJnBQRMiI4JJ9n9Y&X-Amz-Signature=1289c1e4b5358167cc0ceb6569a4ed80da3106e658bb683af03e888f2a30f39e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

