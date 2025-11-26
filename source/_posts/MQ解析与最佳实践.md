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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SEVMUU7Z%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T030052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCpgMfU0SC7zmhsVee7hgAVyj3kJHJxTciErXnGrx7yLgIgHOug1NYeWazbtzSkp7qchamAkU2Pe1wZTCfwUloogZYq%2FwMIexAAGgw2Mzc0MjMxODM4MDUiDMTrhHGkBnGbRitkCyrcA%2FX%2F942yHV%2Bp5j2YAkObrAr1rbr4s6aZmdvwWQkL%2FzTMIMEjpRpp6tV1oQzeWRlyd9TmxsUDaCqsX0O71g3HqqyJ2SRX2JcBo97F%2FqG0NzcVSeCSkntfrhlzPY8EFGpDEWpMAOkK98hJIiPKQ9ZshpdBc5UVRCwoJDALsRBhZRmQst4zjsOzJ364T%2BcBNtwFrsx8nfwQvr9GTmrIM2rxgbb2OURpso%2Bq5LkiSWIb%2FP0Is%2BZyzgdmBktJPusnwPyYxceEJVSX6DbKxuvj8KDbPzyzaoO1W61aNd5d7WQiBlya666Y1G9vCN9aSX7gV96vdPEG2vhYZ6LtOsmhCBb4hMur1vDgG22V1fHb5m42zcadXa01lwG7xjzg6Igdu2Ql%2FbHCEaL8Ja0Nrb6yjqkVZ0OWmNHOikMrvpXn7agO%2B6rlkB78IknwzzKeCyNA%2Ble3D%2BvkWi9l2BzYqaWY6%2Fd9XaF9Eg8rygFDhH8ldPdC2p3Ruzh6rj7f7dQrNzSyHGsreiEbAkbxlaVK6vlzNv5ktMBWQ9%2Fgmef5%2FqVS7X%2B65v98Hs1xOYjmUaq%2B4Ntba0uqMNolpvDbseeou1WmjVWxBt2DY4FQx%2FI%2FsUq%2BQtF0BqJ6xQqyxVZlojJYI%2FkQMOjImckGOqUBuYQq%2FGdFCUgztiYU%2FD363qivioLG4PAv7kAbGj3tIB5Xaph7ODNtuucsbeNNF4N0JzVLE7hz32YHdAuGxIXcOvBqaj%2BEDFYqnHQkC%2BCVAY6tJ%2FyC72bCW1crvAozhKHZO5pnzNsF36dZ3hIExdyKuOHtqpcDXKRkUUYooIjre0Co%2FqryfiCVs18%2BFsVVT7hrq50FcR48Q7UUG0BHDqeuYXKEY%2FG3&X-Amz-Signature=9d4861070f2f44dd16a50b3d70e75e94c4b9508267359000886bbe2242cc3445&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

