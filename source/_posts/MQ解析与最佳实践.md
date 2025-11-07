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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SSDYL5YU%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T050049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIB3jWRZKpTq3lt1bmluElpUN02wIdnsvjdrR2AAibT0tAiEA3VPMeZ8nVeq%2BGMZnWW88PwLJGdN2stRfiyugIa1BSEwqiAQItf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDDTJXQ7kc6jpbx%2FyyrcA0jdq7cpu1WCas1c6bx%2B%2FYqSqGQxEHPRHIGsuj3hEhoIrJ%2BIUmWqT87Ab3oYrKfDdIyscYDCP2yIacbXvzVgmnVx7HNGD8YP2bHHDzNLu%2B3cIxHcRigDO29wjUOrjH88tGfDq6Mg4ywedAzM8nBkqgHaquHJJNEjhoU6ATp9nVvhjHSghg2EKfNieA56VGPmcJ%2B0KJOh4Lksypohb2Y%2FK%2Fk%2FQDdlNKzFeUrob6jpwOiGDL%2B9v5nd%2FPCpSas0%2FZGp%2FWDEbQHthiUHsTgHitfK6YFowtRKI7IzfXeN5u2Bs4TXTlrLVc3nT13HtwRREuuo7ooPHJM4rkNujK3UF%2BUygwF8MUVxCp0UYanuLCqXTNeEn0Ws%2FlyVuTEe0W3G3qmjBTZHkUOvT1JAu95N6ByOxSvsT4WfaI22Syc%2FyNf8Ruw46kj%2BnApzcDunYeV680QChtKuOL2B73EINBJTDVi3DgexkeHHlVOqa0VKKxK1cpBV1MJo5IfNtk0T%2F4T1c6XCFJ6nrTODsjKlTKbOqn9xk2yS8xGI4EpjjAzFtxljYvPlXnO1u7yvBgOAQprtb9o%2B%2FET%2FScpr6jZ%2FVorRx3ufq0gN6x3JgMNzcGMhTD1rKh9jjlocO3U4KQyQMIQiMKTdtcgGOqUBqnbKPzTIUNtz0Fo56aWpUVAZkBlfsdKHKvo3iA%2BC1bOjzWfRx2Xx5fBthLc2ZSwjLBNN6eRb13zZbhA7ho0ee%2FeyUPcVe860fu4MPtSxEt%2Fq6IYN5zaeFzNNFv87SVaZYcPvwhzyBBqxjTOcFRUFb%2BCclznbYFb%2BNuI27TWHoNWr5JuMPr%2FQOBzVVMSf2NP6l%2BHp5O77uGuGnfhVSx1xJFZqPdMt&X-Amz-Signature=a383ea820e5e5adf261c8f1582bdb464632c3d838c1718339016ab28e96538de&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

