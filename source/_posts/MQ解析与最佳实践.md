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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TETYLR3J%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T080044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHgaCXVzLXdlc3QtMiJHMEUCIQDX7OiQUm%2FK1%2FOFH2PN3QSAd8QKtEL3lbVpGq2PewvPvAIgXFFlWDMt3ITS4cIXi9j6idhaTIfc4CIYYAlAZAldBDAq%2FwMIERAAGgw2Mzc0MjMxODM4MDUiDHZZx9FwKXFUaGbgYircAzJRp9d1mCgxDp5vKF0QZd9uCCkwMv13vqhW4Uqz04AZgzYOtlf0BC1rKWU8Jb7%2FhDFKAJGUeKbRzuiE03hPHE2tF2HrYJq%2BgOIp6cUEeNffSrqjmB52vhE3pdrUUVS9ft9ILRmsLvIRTgyEaxXWAg%2F0YgiUrCazDw95FVqqF7UHCpHgS5P1cRynUupFUkoAiBTX1V%2B2GdzvPIJ4zMLxYQJkOncGxk42KWJLyytQ3%2BTdTNR6VY0NZQ8kDx8IJi2zhC8XARchUWzVFYY4v2PqSNFRmCh%2F10I2rw%2FsvPRlWvy4%2B7y0Iu8t1K4itngxFzNXrhviYhJ1QqiMDUpeC%2Bz7NQtrSgKdzmjsLAr8uj701AlVGomntfidX9JbFWC4OhBuc%2Ba8Wha%2BXu%2BWqfefRD52IyQIFNn%2BYy%2BdtZRYfLs94L%2FnFjbqBWalYA%2B6FGR29NBthxwuDLMyOajcByW%2FOz7G0FGKM9I7eB4qutnOIeJFUm7dfpKGVGk%2FNBUc%2FBzBCcDpUo1NbdLt2YOBEMAV6bri%2BpwpLEe6z1z1pMLM8me1xox%2F%2FcpGKIH35Z3GfTg4WbPxtxsEjg%2F%2F%2FoscJGcmauPtpKCcWc8Ua6JejU4XqAtcXmzcW7bGcZaf0P1wS%2F9sMJO088YGOqUB470twK7Q8DtEFJ%2FGcJrBwcs681gareqaQ4KN5CBgpMU4KqyqF%2BPWhXvECPTg1xWS9TsjNh5i87pkGZ65XdOtnbYLTsJyy4vahOJx6dKXvYrQ7I3f7rY4X1%2BVtOjkpzbisCS5XR%2ByJMld41%2F%2BRDdjBL4m2gguLVFpn3Y2oxRweZPBOpHd79AcDUaCySWIHDURXTPRpr4yCUMQ3pULNQm7dzEHzEva&X-Amz-Signature=c4a24cb9ea867d8e35ec584329e09f056b4518097578852a2977c673c7341c77&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

