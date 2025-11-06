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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QVQB5FCV%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T020042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCWi4M7Z4YZk8R2fuidzS%2FCYTrEfiyvRs%2B9M8W%2ByVqO%2FQIhAPAFK%2FgcxtELVnN56EFaN4tKf0WA8F4VIIsf0oQHycVCKogECJv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwjAPm5Vun2EsyG7Pwq3AMRT%2F4kjUZS4cBNunTJgfXilRqQa%2FLZH%2BveLTE4IVhnNdQACd7hmArfzH9M8l%2BuLz5SM%2FeJsOiHeMlULZZclLs2Hsz0Qld%2BLCQvOXUpjjGpJspdE9sbbgL44tGWsMdv%2FG%2BpHTAL2VPoduV%2F5hIlUAX6F1dlhmoTOJKd3iyVX7kfgAAJLyUWPtd3erU%2B0MjrQ5f4OBURvMgQWi2kNhC%2F4LFIbtBAKhizYhYPWAd7E0Fo5Jh%2BBrFCEwU8eLvTXeVFSP4z%2BRBq83gGcRgpTcXZehowZEPj%2FNbJuj8gmwMIZn2b8gmQ1BB%2FVCotLpxgS2EdFJWtADkGvRmirSAHkrnv3gMcK4ETptUW8YCRom6NhkiWbXkwwa2Sc%2F5abkLqRKndTWfuPElVYvTSWW2KP2bdp0eawRMe%2FLPfDg0NFBa2Za%2BW9MYYMjxtIjPIptvnymRadPik2aPQhlf%2BCgIHNEWx4xUEGd5RuI1%2Bcpl%2BWT%2FRqCcI9CGPP%2Fgii%2Bm35WMAqUdcGbtd2q0ez%2F3z9kvI22Xy9xhDdAC416oZheXKSgE5JvdV7nAmym7cxMykFtkqoFCJgFSBLNc8jqJgas%2Bd1cQKuRJtXZVbrKkmdLORdLEZozY2Sap9AeEqz2TeJx43NTD6%2F6%2FIBjqkAXgmf6E9B9DirtiG7VcHdNRR5RwebzMUzUU4U0CU6dMiOyNq%2BR7AJOsBSr9LjxyRNRSBs8U70cYCoWiZhSB2uj1YmQiiGwS3wWq9fmkK01ZeqiQUzEKwFa4Yh%2FQo4nvIbo9BVTMwIUiPqrEUZs9j0ePweWXtw8oQ7Bw9MF7%2BQbwWf%2FNnkhmo17agMYjUEjN083gEMBHyqSMouEtirUDTHQaNsvxB&X-Amz-Signature=0327c3896a350d7e76a01d7a21eac80ef9998162575bdf7367b7dd9a71bfbd78&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

