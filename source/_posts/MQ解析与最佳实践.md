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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WHOIBM44%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T000044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIG3uyc0xtU%2BVIj3nxNYDv7SyXL%2F8WhGNVcaAIRRbR3wxAiEA0VwdtdDjrekx5j6FQVklVUmz6H8b7P32qRcz2NUWvz4q%2FwMIaRAAGgw2Mzc0MjMxODM4MDUiDK%2FK2XGMUVcVmOoSdSrcAwIPiN3%2Bunhskp9Rr%2BwaMIQJRkQS7WnZ4hb01uPJQjh%2Bf%2B%2Fs4x0OTbJsyBDTpbYnLPaGDvL8W3SAY6tIXw1tYxaA2XabyiVtAmRAoQYTtPxSLsWc7Hh44BIrCfp8NC%2FVcSbqyUQ0TQejsIGRGbC%2FSB2Pm2DhnftVo5wb4t%2FX90ejxe8rZEVD5tf9%2BPx7aSFERPCphHMqDo4pkgyd%2Fd6VTB4%2B9FFS4bV0Dc3krCPj5aMgSdNUSjej%2Bj6SutiwR8D2GyRQIBkhlpiVYOAI7esT8PyZ0vsDI7Gx%2BUxwSxO5yIFGiL%2Bj6F2eKwVqs1qCIECYzssfHGUy0Nfc6nlGcGUF1oE9mzrUkeiisKPXMeBPX%2FJWhA2AkTjvbztGa38SQ%2F2wMh8gqaTdrUip3GtUavQbqIXm4b1x01WVX8cv5t9VI0318rJJBz0vW58d9Bjx1BenycaYXrS2NEbowk7owL6UdP2S6S9F0lssRHmQ%2FNpfVNJEWzcv9XRdiajUxmjL8nA2kw0rrpGDLk9J%2FcynvdpaP6MRklKwv9Icxr515RhlHwGxfQa9TXnzwLMttuxBnAi6SWTkMkObOHrN3v5W8ChvNw6nMcdAzmafTY4T%2FerKjJAIVWBy8Cykw0aFAINZMOv8pMgGOqUBUAWVcuBJlZH83BSJ5O9lt8Q7yYCN9MV3DQT9p1jzq5dNKme7WMO%2BdiCmSRsQwJMK2eul4S%2FfanhT%2FIYAo8jWJMGzqz9wm3zGDqzUzBmZDq8lGx9lNJD2RADQwGPRy9it%2FElzCRM6oTHcqJxfFbYPbB%2B7W5K0cw%2B7%2B4tPWassl%2FP3l%2FYGVDSeuX3TjS01GEMhsutPzYdgVjmzsVh0GUdgtDESQJx6&X-Amz-Signature=07263c1d73f30c4ee290bb35e0320f2faf7eecd3e2e4764a93a5ff34b06a1c6e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

