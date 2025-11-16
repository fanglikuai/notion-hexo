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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SEWL3IZL%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T150052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAbrtZNrmX%2BvfdTPdDRvRNP%2BvCblND1r5L7R%2FE7JoFlQAiEA25HiC2DPmx8PsIB0jbFNMHyijNglVjGbtYsvTyqMSl8qiAQIlv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLDIT5qgNbMnt3FbUSrcA0MtnnL%2FzHNz3bYgaZXRgFWBe5iGV%2FVzo19kMG0HXUWYmoaK8MAuIWNnZUzNHFi4CbfnuiaCmS14ytjYopuntFlY2O0%2BrauZMO4YWlLdYA9MmtQo%2BGQrepagDGQkfMQED4p%2FB54RJcEQqUPpvynD7425MznIM4Xsuacgj59y%2B8U6tkNOo3nmwI7fktoFsR3sRvoIlknTo7KFmEOPiMRExV8Mfe5GlJRoFqyUpM0zsqpGakObj2ARb%2ByeUo6nPMkTTkYHWkl5azi8O1a3WBlEuU0IeRk%2BaoD1cPbygMvRyNTxiB4SORB8n4P%2FnEerRwhwtCV%2B8bDOm4No78Zm6OXXegXDqevKcF1PI2DN3HsezdOUixg5qHY%2Fa8iuP6JhhjHENsax5AYD0vVG8f4OD2KlORtmpOA6C1xZg3q6iSj0Alytz%2F4CLObridyh%2BPY1apdIAsP0aGHwSSLpO1uZKTef%2F2C%2F6JugjhEhTIWwzlrlBDlV2q7e4hmJCpVsEaK%2FumkTo7L46JMENeC32nUa4AoQXFQHopzu3pJwqmirC7zkIJE9xeRNn1AdmwhzJHbdi8RPg7J6OlQtnfmDdBRqd94wGHSDo9bEMEFxZf3xUmR%2FNZ4B4G9Jd1YmQkZmqa1JMJWc58gGOqUB6tTTNgwGWUsnSDgoWZf3EfSAIj1T%2BEH8VNA9n2VvSJFo%2B2aUyptADRD6n6zvWwR7mQilsUb%2Fpu4d%2FyTm8xpAffkJAJ0SGZT3rJnhwozWugttxyfx%2BLhCAt3tRHOhSbp1b9X%2BYjbuInp6PnM4kn0R0ebPATqmYds1N2hEh2jwTeW%2FdqmFYGPR0cOW6%2B5roYg82WKqi0kNHZn0QjZMQsk467laI5WA&X-Amz-Signature=be3d5a62b579752f5c4f98b019bad721cc8e42006afda7cbe33000fe39ae81c7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

