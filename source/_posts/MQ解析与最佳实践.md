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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WCQJW7NA%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T210046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBKDXahPa2hzvGonSdwAxPWSnCjYIC6Vw2IeC4qkweCYAiBH1TXuHVtiPS8p%2B2oUg7tRfBdtOWyQlVGduUfNs0YnISr%2FAwhOEAAaDDYzNzQyMzE4MzgwNSIMYaOF7yRB8ELLqeWbKtwDzxPxUB1i2z0sDGl88taK4Sm%2BRyq0ZNjKGWcpNT3s5VOpxhXV1q91TjzC307UK6kb%2BFE%2FzxcCEEvI%2F3CKGh76%2BukJjdCRdfV95P91QW9P22D4cdlyZLWwWHjDIayTIoBYBt8ePsrfTscoTyBNP4n6VElumeKuCFJjPfZhITfBV9DlcgWro7iJvCd1pSteW0DwCaiP6YTpn1pqif%2FUxxFpnGNBkTwdHIvdVSI7GlRMNyttCvpQ1vAxyqWpm0LMWqR9uyYhWPsKdDXRejo9kFG0inHfwYz0B%2B%2BlI25asLukdYucqaRO%2Bea5dZ4jCvRssOGzjHAg0F3sB%2BsAnFHF9ZOByTq%2FC%2BgNCjojAF3pHf2WzejrKGi%2BElSdf73Dg%2FPc%2BlIM%2BL0VqFASZ9PgN6xU1aDuT6kdu9Hjyb6R8VYUehBYbw83o%2Fm4MPHfwnTFDQeVCVbhCVT8YzdIXIasTC1JNRftGBRN5Cu70NpLoCZ8dz9CCW5yNN1Vyj0YWUzNHuRNFuK22ArS%2FjYLQakiZp1K311FCRpeo%2BfJvQcyjBbuvxK9oK%2BUbc7ap3t5zOUc5ycRw4rQbGaVXWGoTVSy9SP42c9wXL3WlZqvUzRcKWR1hgiq4BFU4fQP7Kpp8Y1Hwlcw66XqxwY6pgHIHFeVMcBmsHu6gTAK9YMEM6bzFkuYRedIiueNwCXx8f4co2o7CQ0e%2FOzIXYv0Ca3gmvANBk1pSL%2BeyQ6JV3H%2FeXBPooHL3EKtONrGbaLSF4SZ%2BAgeSnDcQxTVkLOt4sc7nUs5E%2BQeHv0Zffppj25k2f7mwmrC8EPYOKTSWmyPQBIoRxHYhrmJnMAbXcLX6rtGn3c%2FPba0oAf8JRXEIQ9gtCwW976L&X-Amz-Signature=49bc37e930faa0be18e0f7d6ea92e51aa307c66e199bed1910013a157ae24dc6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

