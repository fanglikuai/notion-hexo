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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667LXZBKQY%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T230045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD7JlhedcaUb2LqYRjBhtYcKGZ%2BX1%2FXp%2BXZnecLmV5%2F9AIhAL2MxA1K5%2F%2FuZN6Xk2Iz8wSY%2BJyVeV%2BvG9SwyD2lzWuwKogECLD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyPRo7BSLT5pBuWJQ0q3APu%2BfgfpjiYoldaXfu3UHrpbdXvKATJhcFukglNhUu4ecT3T5zf8GWFpkS2qoQjtFDl9JlsxSM6Ci24XNeRtto229w4xobHM1RWx%2BeKgcv4nr9igK5780I05buPogAF3m8PHTqzTOqpxUZnXKpDr7ThLTeCf6ZTazPDGQtLJNxyPWV07cerlWOQddAtCw5xnWLoUeXO2mOTEBYTmhnuRQ%2BAUSF2xqEhLxs3wR86bkmE6EGwqPaUrivhbmMH883UJqovZhjj43JZN1SzBh3liFe9gcVmEO%2F7AhmkzR7XBVssU9VIhTMyujd4FoFWIWhmjKrmAjpfvFaxXqaRmmyuMtPxMVTq04%2BiG3kFh%2Fr0bbF9QC%2BhjwKVCGDCVPn9fzQiC8nkVJ0mo6mVnFGkjvG%2FpuuDEyka9dacmelgHLwD25m%2FTvlGs7duA8U%2B%2ByKeXlnWmLOgDL5FtA58EHxCwvI3tJjf3VgixWAHCMtK1LBAVYYnMbLfp21x9UaoTPPdiHbFd1PaJdw077RVAA8zVavNekGKGbtASKln35Aq49m1UaV9p4SHevn22Gs0jOTpmRqe5%2BEqkzr9FC%2B7KdPIillV9PSnKST8vfqfCPimUFPxBzy9EPdFnPByk5ZJ9mx%2F1zDKxLTIBjqkAZkZArA3NTgD8WN%2Fca3cwUEMJArKuBACP4U5%2BwkHHsPtWTTGJDqvoU9ttTs%2F42uEkbX0ePGWXkYy4bzezSDprmkDrOdGQmOFpK%2FQX6YUv4gFkmtlk7vMgLBANA0HnjlNOBZHuq4n4bDYSSM4g4L4PY%2FlCZD1tQeGnjaPKijIibicGWy9a%2BFr7nV44xUthWYYMWkyUwVvANtwMb4Mlv2BhVdmTfEX&X-Amz-Signature=cb2f1899abff519418a1a8e16593f344c7183ac92d1173cba26570d70b6004c5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

