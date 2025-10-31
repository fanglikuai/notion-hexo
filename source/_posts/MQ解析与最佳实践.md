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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TON7P7LQ%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T180115Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFEaCXVzLXdlc3QtMiJHMEUCIQDhNcG4J6Je%2FXJi5lcXTIhaK6uh%2Fxy25E6Dcx%2BGErJXdQIgDzIIlPw7VryKf%2FMm8wvjv31DMwceqDec2Y6UFqkkOFkq%2FwMIGhAAGgw2Mzc0MjMxODM4MDUiDP6d4BIhGw3vXvIKTyrcA5JU8B8t8Mf7yUb1M2SPlPsGQv2kMRWiOZnzESHTsV8LpR6S%2FOgzWcoGuixbDF%2BCudkjwTmUu2ozBS3DmZdxZ%2FbrjOAfzj%2BgsYrTGOCUka9Ltc256T5fwndWAvclkuKHxAMjotcaNJuw4a5C2udrJkXJrGESOrwLpmKrloEQfB1WQHLOiSXn1JccsZVG5uGIpJjxB%2FW87Tw6dv3Ex0AOrOcIoUvmF%2BWrXRtegrfWO4RoHlrFCXadR8X3YUNuJ%2BsdGU8NvAS9kHmJyFB7Rb6JdMkX%2F3hhlf0NvrR1Qk%2BL5OvL5UgWnk7pZAQmsT7y4CHWAtoMTufyAJcQpvveZTtX2JNkW6E1ND4irTI2NuIZvO7zMh0omQI0qQSSODklnR183iTdwPe0Al8aO4n71M21tFJJckcGY6n4Nlf6RTggB4IHcKbaUi1o6MJFWRH65TwthoxZgF8fWvj3Vfd5ZdDoNRvC6U3p2487DLv%2FgXHQUw2D7vJTJ9HigNcymDd8l7Jqm8HkxiEQMMvPj8M1IYAXQDVz9GkodayarOIH4CY7rRYIAHrjBwSzoxa4okMSc%2FQAPTTq8N595ujCXO6%2FWsB98q7MAymyaUXzjc2eLR3FtKfuDTa18e24B%2Fnnqb6%2BMLbck8gGOqUBg0Xbze5gCfdbwHjfuLF9x3C4zxBM%2F%2B15gyMLp%2BLdP7Wzot3s84LITmI5piyGaokKLgy1dtv9I8yd5E6gGFO9AZhKt3C3PbTg6Hsyo41QtzGQ5aycdhlN2byrIdyZZyCKZhG5vHDjLhtb3mXH7QpX3g%2BJVEQBPp%2FClBzQdCh1%2FdJyD%2BpNWQAyuPmm3ETtL9h5cXQckS0JpdkNLtLDMp%2Fp7cNJTxrO&X-Amz-Signature=15ea00a9b23cbbae97fa8e87cfc9b05e352720f9114e040d594d5e8a8bbfe894&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

