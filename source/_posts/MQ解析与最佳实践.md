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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667FJWHFPJ%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T120039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIA1AUIeHK9oeLVpJFgKVHgeXbpGbqYOMmdhMg9y5FviFAiB%2FnUowcuue1ctK5DGDYpuuoukjy0QtzEPmXhKWRyVAeCr%2FAwhbEAAaDDYzNzQyMzE4MzgwNSIMcV4dPrwz8tl%2F%2BGLgKtwDJZ5A0DVwwW4RycvCHAKxiIir%2Bhm9uTx6a6GPNnzaa7qXELmcf%2FulEiE8fqc8fFzmfokT%2F93xqZhqag77XyDm4B3iydxk7RZTrza%2FKaEy23tA2ZN5zqVAdlatOtxPFY%2FZzFck53OFebXRRdXlA3hg%2FOSsKhmGsjn81qejK2viunN1fFd2eMXHq6DOVnoH77DvsMmfOHLBLT%2BRsb6jjHFINSNw0Nu3RFIQTGf3l9r0b4ll6chhBFvW5y9Ai0imszoic%2FjWEHnxr3L2kIdudbHkphZI1HCwwQWWplzHAn7tZrn34r9MKCTt818%2B%2B8E8FHXuW0gERAGbZITr02TQcfc6yxZCPuQ4QVJflQeItWxjBpHEbRfWnhUHxnYERCefG9caR8g%2FmO7i0o7LmEm103CcsBTCKVok0wswPKNRSEbB7jDbhpeQ3O3mxE3n%2FLIYvC5XEv4YzOIpInBI5pNUElkEO8xcRPJXYXaE%2FK5%2FD2RD9eHwlW%2BQosgJVQ1jM5Of0DAQ3t9ZdvSh1jbIGQzwilqjE6w5pA07Y8nPh6beYolAh4rxBkp1mV1qHpwub3MH%2BDZaR3H8CttlkJ7S1YsYdKa6VeEbA0iRuGcUEA8wrZL%2BvKHKmvSQ6m0rISbZaysw9eCDxwY6pgEJPDEO6OE3aYtAwipyA7qTby0o8pPzDUajyDeeZaQy6%2FyIcURjbhc7xhgg34Xr8nV4yyVMsbNqg8MRVSZEdb96eQRQ30z%2FvFVfmKEAqYwg2f6CqSiwyWXfoD59bw%2BckGuMyTCE8tZjy8LVprCz3Sd1PaNUofPrcD2KDjwpEpCMLEHvW6a7nqFVyYsZPtlNxXsdSf7JM0L%2Fm5gKhmhuVKbINQ6i3qWX&X-Amz-Signature=7707701df0e197e085d8fa4f5ffa472317e901077ee86f7db20729256efd7c43&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

