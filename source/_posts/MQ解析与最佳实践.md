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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TN27GLPA%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T110049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBmiX%2FJvtU5p1AmilLDZW4P93B8XtkOX2tLdHNwzwf21AiEA968XfSpNq6ohe%2FDtq%2FaIFhA2CazWa%2FF9361%2Fo1WTs20qiAQIof%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMaqT4vqxAwFLeuwAyrcA9Kcddjz4vFGg4BUrmdrzyR28Af4WWpf%2BFwKP5sRpLDXyBKM43qp6FwKIE4OjiMMY0jusxJYh1j%2B4bDR33qo9n0nxG0%2F2hF3ICYsF5TtN0hDENg71B16M42J6C5pSMRuVJ8P%2F7MQ%2Flz0U6nW2BwKhdQBG8l%2BIq1NNkLMaq0wdfftw%2FzcEkyT0EL5ZWIt0htFTTG7EJjH5JJmfvdarxYzLtkQlo9Pw0kiKHiTAaNB89mHzlLves7ZARuZbzfRPo1cy5hoA3kivPLXd%2BX9vSCJSpmahXOwYrvqpNvXPmIDW4YewWtSrm%2FasHMd8yTL9xTFJ0z01Oh68EGgcKvlrOZgBqwZ7MOhIQcRxTq%2BqZDbj1bqvcTUVb0xvdBGA3mT8jie%2BqzRm9uIYaAZ0emQyIG9wNmPa5nMJY7LyjAwkzV2TQQWrk%2Bq6KFYAmVMSKXoejamJU68zNfDSS7AMKk4hrgVcaDnvTV7bT5kfXnYNAECubHNbF6jJctKijUFiNvORpH0g40wbnCH1JpZaFdeo7EZJF56KzJdBZnaFQBGNYgvYh1E0puZrhTxnlFwLQwr7hYr6K4SpIRflMxVcVRQby9nIlIyERc0TW7rihX4UGOj9Kklk2eymBMJ1AHWMZkzMLnmx8cGOqUBzuaahDlMWIUuzoPAXEj1u0xaqqg03O5teN0FqZoV67ahYf8DQSueeacZpZgCacj3XmTJfCIBFl7SCfZ8Gh%2BNeOtGS1j1JDbvyuPFMBHi%2BRw9zqFiTOKcg9Vm9UfANMwnguF3L%2FzB8OySKBSqFBYKkR9%2B0Q7wVyML9hQDEt3A%2BT4QGqdyPBi1BBDTIoFGBJeKpHQO3sIlmIrgr4LBD3poaQJJcdAa&X-Amz-Signature=73115e642124302913f8106c7b6bc77ae937cfe31ef681c966413d345bed883a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

