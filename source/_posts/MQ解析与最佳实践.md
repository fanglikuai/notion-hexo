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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZSO6Y4NM%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T120056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCFLsDMkxZ%2Fp%2B0qi%2FtmIr%2F5jO3bgSc5H%2Bs6lzKLnpUB6gIhALovxr4yupw2b%2BI%2Fk03QNi8XMDklIbiii8MUS4mHUrC1KogECIj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igyko7PcjKHoOBh7l1Uq3ANoyk1iRlnrmVb5vyGfC0XvRidzTdT1hawJfC79TjxsImEDbDcNIUG0vr%2FucRq6lQvMu1TF4aRS2VElqCxrZ2yh2neKWHx81C6P%2B0eysoykCof6yJO8Ia23ph3BtXL2eVZLOA6VtLWCr4CYuGbr49tx77QyCcqsHdAfJ72DEggVHUhFTVvvPFR0jRCKiuE3ACLDbOrmNq9tl2NMEsdcF2%2FJ%2Fh3pSAEHinNUCu82RE1V4bwju%2F7TE0wyM9VIGgUdyBi3Y95TNejxySzM5JWEaw8STbd5ytuGCXGVi7ubREq1dQ2PJZ8YWL%2FM2tDFxXfdW4uu3VvEGa8oTZOAIeeddaEgbEPqJd0%2Bjz5nYyaRCIH3vhUx3yPxmhzFW%2BkgsKnXoqAJ8AwNB9eJmY1qxV6ZFPCiw%2BAX3eeampUxZQDrqnqhr%2BKGcRZN7FNmecBb3T3e7bljchqW4gTJJ2oI9szv5khz%2Br31s0ku7Aroc4N8VAyt6DSkYpeD7fsdbxcFkliw%2BPQeSvPHUN69GsUPgw7kJecC8pcBKRjbZKJ7lun0O5sgPxUqPjl2Fm1KfNhynNFL%2FuySyNGzl6pGe79wVK9fNdVuY99l%2BUgxB95gmOLBB6K5j6CgKtGMGy0x%2B%2BkfQDDQ%2F%2FbHBjqkATghST%2FxJlKTLdc0%2FzBAnz5%2FT6DaJAMHq09%2FBxIinQAeK9xsDXhWZnTccKzRriG5heSwBStuBciWoEqbmtUiX%2B8gqOs%2BYi1grfcQ0LMW5VUEW74tRXR8VjplsQm2mEZt47qgLxcRT4a8D80sNteDyfepgIQve43w6Q6MUl5g3Jtf8JcRu0eO5E4eOeiSpefmwbWy9UomGKtpEbqcfoFQg8%2FCY36b&X-Amz-Signature=9700270eabec341ad408fc73c53ac7cb183b150eb9bfe2161d840780779efe93&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

