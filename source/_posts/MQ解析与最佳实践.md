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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YCJCHDS4%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T160048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDWkmXgyrBwtpOwsBbRUED86wpdKDiUbZcy5IUgujWoRAiEA%2FfCUnzXb43smiiBpuTsNu8fZm906xbPbQFQ7BLrEni4qiAQIsf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIjVmYGG9oNhlzsmXSrcAwTPKsmPE6DWye0B9o9RqK3%2Bp89EFiZK5zogZwbWoQdnLbW3zttlkoLhQMbwR%2FZgIAtJLWrb9VH24cYKSxYlt5Hgo3wkukgAjf4HgEN2PwXRwHZ%2F4jYmC0N89K2yF3in1zG5K9RX1bow5cdNkTEqNlXn0TBzX1NlhrSyUYOfdoJvnpZ6yzqGQt6RGSY4HD6BkRsWhWAU%2FN09nKoay%2Bdc33ml09Hi7Rm8NS46opzxAznJcxIq%2BvFjpvk7mz1I7X1oyO70%2BHpOf1354vmrhcAcrxCrQl6Mv8lwQePUOvSe5d8%2B5QSu3GIXfR2CvzTHHP4eHVcG%2FP3zAThfURBqdjjVaDF%2FC0RIr3G22XJ1OcNYA49d8OcjeS9R7ER0jbgfyv9eUYVFaSHjdb%2FIx9Mv%2FzmGnk5%2FuoxMmXP7JKRfMKw5Z2DVZrYtWD48Uuic4iZk41x0Yjm4WT1Su8YZaDS6FpuVmmK7IJDt2i5FWlgWp7gsqWi%2BdhYQQAZ%2B6xs4PRV0Kpdb4oD4N7qdzXgR6rgDyQOOSbCCfrdO4AD%2FEhOSapG2GZLF7g13df%2BgaRNZm6lMRILeZfjdr1on%2BvvVkQX6AU2rahQmvHjJHqVnA86906PQenfr0gO0H7tUTpT3ThCGMIeJ7cgGOqUBW6Tq%2BL6JdGpq%2Bd0Htqqs0sRIeJfWUAYl8h1bQqI%2FK1lAGezDPUpNf785HsNP8i2n7WNaSgbcLObhqQchbJxf57BHu%2Fh4qd82%2B6BMWumaKB0OLOm0kT3VZTkrvxO%2BHbMoWjAf7Q8oLbjwkJUZs%2BhpXJLJj8v9C4VCv7osX14sBctlrYniB77%2BzfGLskdy%2FpML5SnPBYsYUZ21WEbzbj8qxFUfPTHw&X-Amz-Signature=fa8bb58a2e788a2f814d7595a3d4bed030e38496b888573bdfdcc8148b8f8e5b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

