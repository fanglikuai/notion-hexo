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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S6UA27HN%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T230054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICdbc489tu1lbR8hNdeek5xNMffnLeTYUp9nzFdvTz3TAiEA70GspxJ02NjnSEM5uk5qYi91FGh4hWcpUIRhXaCST0AqiAQIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCQNfuqDicn%2BoKkhjCrcA62Z8uSBAzIsQ8gw%2F5a3kYqPCZ09Ho03X6xxQnBMAttKWlRiRMn0WaPs%2FwM7sLCgsfvR3NNbDnvqJ0NzW1e9dl1ZTVcyAHhK5Vb6ymYkpgt3u3bpiDD%2BM3fM%2Br7a6cFcpIiJAgYfi2RWEV30Vm0GywUbVq09Gd4%2BZsiMTt5lcMBurk%2BHnE66lgR137qBQ604io2Da70U1rXkgi3m2GcLUrLKMUlt80XWel3RQYtav6cE%2B70Sd6MofB%2BfxTf0QLoJlD9ymKNQi%2BOpNvfS7i%2BtZL%2BNGdQh75CzaXc3wDCMav3mIYUSe%2FBOADWjU6pLoO8pWUrd1YR46XDWRjuw6UbfWrKvE51mQfHegyD2g%2FDg6I%2FH6nn7U3zibjbFhrySbibEdZj8lE%2Fj0RZeT9v6KQ3VrNaD06pU50ALyPrPsLzkjoREfmX503nXZPvEKX%2BqA3%2BUBAM76yShV2LN5XD%2BDcYVPgeUb2H0u2dmEgxjW0Q41rXAnKt%2FSXgbImMBv0VPAUIF0%2FWTZuuvCEGRV2tlTb3KLCs%2FQKdynffRnnJGKuQZuUDsi%2FCa6O0G0X%2BjodMtEW3cDjH74opnZ4yexZvrYHtqkpQa2dq7putqZnxct3OcQw7W%2BvC114xy0NaEr9aXMNOR6cgGOqUBcrT8oE7cI3qe9gVsYaebYj1sro4hjQ2qh9st5ZlCc4T94t6OnRzlE3XFI0giLUW%2Fw7Sli7LPC0u0nS04JxIhMdddm5hhu%2FysqYwza9ho9wYrsw2UlkTPIj0rNk%2B%2BVb794oooe73vPKbZ5H5OGHCTm8XtI4IcC4kYdII6A3OB78sW%2F7jIwXX%2BYJUsccNjtTZqGaK%2BFJXncxiTItVPzXJ4O97cXfAF&X-Amz-Signature=1c0688be1966a57dd23706f7b838ec2d47fc1578b01563fa03c63606ab5d7a0a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

