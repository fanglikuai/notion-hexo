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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666FDVXZBJ%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T100040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHoaCXVzLXdlc3QtMiJHMEUCIQCv0qdP7TkLTbrEoQb8mTvJrLzwIT%2BzAeUvqryKdKx0SgIgEQcYJuxoHO%2B2eKGpeqP6c%2BjWGX75r6uUQmt6pH%2Bi38sq%2FwMIExAAGgw2Mzc0MjMxODM4MDUiDA5ceMwyhfSfiKpWhyrcA0en6Qw%2B9nHK83wxf9dJoJmdKcx3EtcoZINnMMRyqhcvdfT70vt3yA5UPtkkst%2BITxfE6F%2BF900UDnnjKTkvxX4ZwT8Vhi4uwa5pM7HYgNiJmXVrxfHkNMRMFghsPcmTc0tD7PBamqUN8AZoXHB2lrg01dkmZdtChR5tHoyMLt0h74PFpkp11KFWGQxp%2Fi2TE14V0ZXt7a7BFiU7Zae4xgwuBItvMR7tBTiEmimg0WLR5b4fW7WHxNF4B5XbA5kT64ntG844hPAwV7zsh4%2BHn00rm8sfm1Jinl4aoHNxOAMLSo5wCiHPE8svP0JacHQl9RUbkgcbogzjejUIX7sf%2BQIzHI5Bgfshvpkc9nUq9LH%2FHFQEO%2BAhJE3gJOUp%2FTD1Vq6rqWehSb6uEYexatboOzOpYILYMyqq59k2w%2FLbqSoQq%2BpniStMnMMs4BZEpS8EpbspEhs7X8I%2BeDMzDVzWgRgGf75ItVsSeDMgSkm2sYqvlksnTvdNiCtySC4KQ4%2B%2FO61SZn4TuLad6tdriekJpI%2FgtnljuHMTmxnpo7kurmTz08Yna5c%2Bq%2Fi8eAzMmByY1W4JUXZyaNf2UgcbF03LFflZfUyXAxdG3MaDtn62YLW7Y%2FjQ4hYe7Z0a3NE%2BML7t88YGOqUBP3uDSVFsdz%2FcrQ6etC5hfIpP%2Fz8e3CCTRce7gOkt3AdZFDWRUxHalGfmFK7T49vKwEgRzSAc9Rw4O0CzFLvWnwnPJHlL6gcER3lro6w5Y%2FuQfttcnTxDl3nKSGXXCpUcb1E7dOFG8uUA%2BrNPrM6rzLUyahnnykz7cugmMkg8xjYSJw7%2BNjla%2FuNqizfa8NpUfBZnpyPUu%2FaLnGTIGzIz0gdLRqG7&X-Amz-Signature=fff77de36c061b7eae66c4a36862a090e1c9f4f8972002c1e8a2c3caa0a9c8f0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

