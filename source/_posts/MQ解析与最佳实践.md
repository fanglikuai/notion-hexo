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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WOOPCLKX%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T180039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDn0M5trbXl5Ye5RqKyIr0921Cxbzlmfi8znaHav8oC4gIgWrXT%2F1wtDSTtjhcajn7UUkp0GAsmfl5T4ARCgP4iubwqiAQIsv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDL6zT%2BDxVwFWsqLDbyrcA6UcA7QDstEiDC572QjeP24kA5O8MXGmpzFcnUVco7XpHzeId8S2QWKn3H8bKhczRBFumBMaWV2C6w5l5StlwS1gidX1oVFDBoA8xJiOYpINa7RvtphJFPuo9JwQQ49Sis0Buqruz1LrB%2FS6lQ2FwD0lrDpv8Wnc8QFR2%2FFagexsvg7utCBPTooYPEUObv3z0l%2BxoKzI6K4C4bb%2BDAoJa2eYL2%2BIORWXWehjHgUsa01oBL%2FAQYdPzXqppW1A7sfwoRfkyX97yqrHOPIn8Otgv0mRe1cv%2Bxhq2pGPaIV8j8hyyVDd65uNO2E1MqS7pmUC2VyXz8WaeZV6js5u82rtRzDpWkRvxdw5LrwAysBoIfKH8KXwmOoInfj%2BHlrxqx0rSn1uHy3NxStHdSBb6CeCoEItV59C2q4ZDSYonpgDRmYrrbI%2F55n0szjIz6LXbCafPk8QO%2BnlU4gPtAc4f%2FQplpUZsqF5azjR1DlchhVTnr3SGLPoV%2FvGDQomLcRYHCORSX301e1rBAPZY9OeerNqzcdSHXruaoDEue3U2%2FnRMrMgrn%2BKCGA%2FugF5gLHhYdvDz4tZTocoCU7nUEbiO364MHyay%2FZOwoWHloPMWci8aLYEicQRUiuQ%2FY9ivr1qMKGm7cgGOqUBHht7RCJdRH9hFAiUaOoLlIKm9mRSxHnw3kDd8O0Bl5dfg48%2BsGHO%2BmjJesy8vdt%2BDSY7t4QQFOusaTvLIFyWy%2FTXMbL%2BsmyJl8Ay3iawN9UZwO98d%2BPmpZcrzEXFzNkVa%2Fe%2FD0lCKT7F99e85Zr6VAjl%2B561O%2F4tzYRxeCG3o927ScdT%2BOLqscqT1pJcXWxrYj0ID0AZb3SrqsOU9TelXzeDAaMD&X-Amz-Signature=01651da3164f45de2fdd323e949578ebcf730dca3b687225b66ce16b62770b95&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

