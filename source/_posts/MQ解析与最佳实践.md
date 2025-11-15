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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U6AYBSPN%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T070045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEUBk07B6BDX%2BFhzOs8N2atZ%2FYNa5u4RygsElTahL%2Fw%2BAiBHDIYEmhuQHr%2Fci2oAQLyzXCoar3pfY2XzHTxZ5btqWyr%2FAwh4EAAaDDYzNzQyMzE4MzgwNSIM7wxbpCi%2F6ZXMXWqJKtwDuqhhRvdlfGRSTJ6TS3B0hQqt67IAjDEQ1X0R8RTtr7oFr7%2FBSWWVa%2BiEgVKkYCRz4mEqLADtNZVgK7F9rMOYvRm2fUvwPwTAajyBwwUQYEPWzXWeK7zGw83HdmA18Jn7Wa0Pv08tsbXDnHHiDo49BSgeMDtBJOzHPpSdO104WLE9voMnwrYihSpzpQxjegiop6bmEgRmMOlICUvXYLHAPl9x7JkzxKModOoxIo3PYReDSHVWNusH%2FWHZlOFJcmisZLNCpWFj1vj%2BMIW4L%2BKzklOTXbTdG0LC%2FR9QanBkzbBBXaxZ2tsZvwU6CTtJDBbOgh0Ve5H1NOB%2FxynmY9c9IeFRS2DgUNiahKCS9TZCfax25FT0ZkKkJ8WolD0KXylg9q9r4TiCYKaRWDDV5MdOjemFHvuOnsrtntqyXD24cmJ%2FOhi5Onpghnv7AND%2BDtMHOVRmhsSkhBdegUKC0mbdkwIUs3%2BGaYD5pM8dQj37QJWXkfdQqH63%2FZPB16eNTiGG3LX7F3Ggdy8283lXkhRSYUD6%2FB3n8kZ07HrGxNU5M%2BhbLxECe5kKpKJtpXgliT8VskAHzLcAbz%2F1sJbv8WQhm3Z9x5iuC2SY3ToQwTe7WX0WVZLma0rREk5F6kcwsL7gyAY6pgFbDX537sJrr881YWnGSosLz%2BOzx9kzu%2Flt5iLXC3ee73vrqMHDky3TXBHsjNmff79h5vKVbcjO8OqnWoY0WlevpV5HU9F%2F4hKikx3gGafwMF0aO81KJAh3vi7DDBCDt74Qqb0jzg%2B6iPyRVN8xwLx4JQ4Mb58a4Lo5ZhX4lCg8JO%2BOCYOS%2FtnF8RQ3y%2F8%2FdOO7dD3O%2FEsL1rrgyKK794ddTdFDVTFC&X-Amz-Signature=4df398bdee32fb7709ece64fbb8d1aa545dea8ae0a32fb97f23dd8cbc5be649b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

