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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667HK4RVW7%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T090045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDD%2BPtDuwHITYb%2B4%2BjaWFEnAgEcLssSm%2FfHm84H7jzvGgIhAO6pGVUqiPXK0Z6KfcHJzQpZZrTFGLgHUHUPTEsH2jPLKogECIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzgYeOUY8unepIgD%2FYq3AP5Z%2B0ClROo909v%2FkOGvMVJPb7wZbKqPM5x0ek3fjJyleVQjPkjNI%2FSiDSouFutXYqGS0hpQvPBeiwyiVYKYo0RkGX3gh9GpWr%2FO74Dws2kd6NmDFenVp9Mt%2FGif1hdHeNqEHmGPLsUAHSO5oPZFEF87YjBFC09o%2BdzUJmuDjYOWEFC6zPWAnaYWkPA8Se1inleBtLMTM7%2F8MExA86DgfSIVVMJdWdLAfP%2F889%2FQrcW740i3POCXSuk0SskH3fC%2BykSbQkFK3p53Mk5y7g9jIcl6EcZfgv9WsnCsP7CGvVL4bqZvwc9FcGnecYJX1EEw8Rm7Oov5qnmwzopjSbThEiBqkwbUBdBqOII8WgiDR15wdxKWAUcitnGtsO6VVfgbLZLeWeQWGMEkC5%2FPL9fMEz4mUie4ABMgEnlE8hemsJfa125obMxUBE0h6gMnW7PDnxIzUALRfFlWX44%2F79RwjnA6whMsrVWjVYuWYZeIP3iB0vCiCEw8Tv%2FKBiwnF9tadZYwBYxFU5%2FC5Zcj5v%2BKOQuBSKGeMXj6DEGk1vRI%2BDk7MQpdT7xJoJkOEUvB3463urGt%2FpNRm6MbzhAikXWOvJSdykewghyb0%2FatGMLHoy1yNCdvlql%2BVXrs8X0cDCQ743HBjqkAR0URR7pwfl9OCMhgES%2Bj%2FqDDw9W%2BhTpjJ1ZmgAAJ3xf1uFNALXG02J4SaFmIyklOFdOSfqgKAcA7sgqYb8WWAUO5MrZ4wFH3y%2FKVw0gaDPuOrDR1c6acsPYwB%2FykvgWSJGOFRQ9pIaRy%2FzRA6XaItGQ%2Fn4%2B9k82VzTWOy65NXHJ68Nzb2Caa7uvIDrHJAR9mtxJatxcmN0NpLNDExInjIsmvubW&X-Amz-Signature=2a0ec6d54b717fd90400199bdb532e422e400d9cf122e4fce7fd30f1f3dfd959&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

