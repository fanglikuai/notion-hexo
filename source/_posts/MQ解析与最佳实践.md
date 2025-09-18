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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W6ZRHFFM%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T000043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDcaCXVzLXdlc3QtMiJIMEYCIQCydzWoUM%2F99mpKrbfbR5LlaiDp1kVAOpODQNgN1sPpYAIhALCYV2BYoRt4IvrUrDPJTKBYh9SToc15pdkoxqz%2BjW81KogECLD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igyn4ZQ2rIuRG%2Futt6Aq3AOvYMcbMZquXuF8VNA4weSzaaahywVc48quMQpE7L72bp843bY%2FYU37Sc%2F6ONjApYTgvTv19%2FMlaWswBpSbqaAXWa3AlKkoG7L6S3ZW3Pmn%2F%2FKe3CpnK892tUx9aQV%2FjtPhSqBtutI%2F5%2BnKCI8AO1XeqKUSVTNYXIaHjcXzsS1bZMjB%2FxnMDQTM%2FUMLoobhnv3ZqaLQaAkHvUbDbi25dRGiOX59IvkUtlvJz%2BZszkNc2L14i%2FTceqP3SizeIaTaXTarEHlBWmj8Y1Fd%2BydTkmBVDQcEKd19My99Cjomfo%2F0INB7SIF1iGmisPDQu29yPoyCBz4mbrfC%2FGTE8HiEIj74bUrbijjJ3Ce8J8UsvLG2a0wkzELa7QyHUHA%2FsQzsYIfIBeReoxlq63vkoKyXINyHWfaBV3NYJjuy5JQvbAkxDlkroD4kG81L8bzJPT599x5L16UdlTRej6UMxvP9v0rD0IGuMH18IiOUJe4eB3AC4fCvgpjZZ6jkPJiG4kD%2Bh27QVpwLe6PnpTpNhUAFmZW4Bg%2F2%2Fde1vxBfYrUw%2BXgdLL1CaXJnDJe%2Fg04HS7awlsAeXufi%2BIWC6uRqQfbSgxpBefr00bv1M5r82%2FoJy%2FrUncqIG7tCPL4iK4zBdjCR9qzGBjqkASfZD8RG9Wf8L%2FUvI6%2BQXRVUnEa76siyNlBMBfk6fA1FBYrwDzEScJ%2B5OvrYHlfpSt7oihr47luXX66toYM3ziijZ64eUClZxP7fX83wsZHdfL04WJbW4hTS6Tvu6LxhCDXLxJBeh%2BUMNF6lf7119GiUVSSCm5F1olHu9L4fM0KMKpyhf1MOCiU3j4NNAbwXU306EjRZdp02yXrd1UhcS3oFIze9&X-Amz-Signature=8b66e16d9e56def07f8a89ed84e57801cbb749eae4a05b7c3afb39974a04ff58&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

