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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662QC3RW6X%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T090041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECAaCXVzLXdlc3QtMiJGMEQCIB5f90S9S%2BvDq3WeOj1ZDIQTD02SbWrS7tyfqBXdUj2FAiBENPp8kKc6EVJioAQPanORQQOeV1njrs072uItze4EkiqIBAjp%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMZwDpRU1n3UvtE%2BWxKtwDKBWnFIokcZZ63P9jMBP5Nj5ufW0%2FFTD5ckTwT%2BrDWqo4K0L%2FIflVIMxQNm42YsDIK1IKVbX3275%2FhnRxGmF9o3pK2mxEbDUNtmD%2Bz1FY8zdjpxFQ3gPJMD6oiLbm8MhM7ZduZxnctkGJs5qclNHfQVhtiBkPguzy6e%2FXmvgY8fANcnkKDR080QlswHCGNZ6EMxTxsF873gXvUzS4PqQNyzoxDV8gUFDmbKqScczWwCMDhjFShFwWJUJ4gnsUTVpY0xTq6nRfbq4x7UFujJnb9N20Bkf4F7U0Q9IjQkQk0iKEtfMOwhMgziRh0CP%2F2%2FBBa%2BPV7SCGMEQHa0rYaolW67ArvvrBCq5Loa0YdkpVdKtV2oYKV12C8Cm57NpTYfE%2FAcO9cOYNu26zG91%2Fnit7qeWDQClKxxOEvBN5%2FHuhP9fLWsgKZoHQ1j3IY3Hs7P13IDLH%2FLkfMyOGQhni4fWbLa%2B5qTUvKEngBLmxQPnMpcp%2FJk3u5%2B7gBA40od49Pql7M4zmlo5Jxk22c198DZCdUdMM5YSWQZxY7aZ3pOo33v%2B19RFuq0u8b15ehHmncU3hE5T6AjYkoBDi1eKAZQEpQWwhZCAuJCMXvSjHYFHliwdLSLNjOaxQsV9Tyi0w9ZHByAY6pgHTMzqmuYjOLZpsIRa9we5M9%2FSBemMMotD69qnbU0zwpuWcRqbU4v0nmR1cN%2BJIl4bH5kPZDF8C66c9liVjDHpZnTfefKAx8P3aP8LHvGQ%2Fzn8qsUplUwqJj5%2F9YyVmEfAd68e1mbP740K2fl6QW6AMy2rnLfR4pi5VPrDetdOYtwNM4OrQh5%2BNR%2BCFeGQmLA9mAzzIWSrTQ4o1CO0udt8SNwg4RnVS&X-Amz-Signature=0dd9d16ef16b40ab4a1937bb4222153b6b8eed5571ff2dc1f75dc123414ff5f5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

