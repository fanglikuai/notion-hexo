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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665DNRWTDB%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T060053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIH8VwN644h6ht9u8r6pEkS3QCOGA5r7WUVFDRG6TlgDVAiEAqtELuQLQVbxOKUCxQf1SaNXfk1v%2F9yKoBnnJVm67Q6UqiAQIm%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJD5mb%2Brk2AMMPw59CrcA1KOFnvu4y9sUi%2BUXSLCIAdJblUyLbdzRgN0JvbkGyRczTXNeT4ifEh07Wbz4I0%2Bd%2F3ebA6kghM0tTcI%2FW3Im4iNY6HY20sL1TBLnEdhuHwQtG0D%2BD9gisloh3QKJJNcVCJwzRPpUAhyv%2F03Wd6q95Zc9DjKZ5vPoaBszY%2BXY74VNfZOw9undw4xghtr5SbHJnW5UESLn1Hc3Fg0GTk34n1IU3OxkG7tnJugJyZkGaspTl%2B5Nm%2Fsig2SgdZGTw0W4PmNwhcvKUMHmpFqZO3oUskHJGZvFUmfAO09AhHP9reqK1gBFf9TikrLjCsjoW%2F8%2FRrOYd46rFWuphnzYrrepNeL12WO%2BvXUwR0JxV3LvLv6gbUQGXU1JdxeM0zKy%2BXrqqOBFVbrlLjsusDI6jMQI%2Bx8o3PSqFcrsJc6goroQp7ZnMdwHMuadbNOXOYc3rgX84C951wOHg6L3Md6J3i%2FNWmhwnwiPCCj8S%2Blav8Lzs7Dq8sZt8g3BkMxtUka8oM00%2FDtYy%2Fu8vL%2Bc8IVmRnA4QO1KqBhfUAJZIDEtq4gvXy7Canf7mzNXZfdoszdaPexjPY373vywxBxrE6iR5YyN%2BnrMEJXwedVBgfzTcCdeCAykY%2B6vszwMD8nL0MDMNPBxscGOqUBQLBxKq6q9WipbdTjOKLDXRWY8i%2By6akSd3%2B6FOUiG8%2Fe1dxPpqt49flNo%2BbV%2B%2FVADunV76JSzbR2szvRkEmC%2B7pxdZRIF6lisdeOzIWe7AnTrrRlyoIEWL8guF0bbplX8iEfA4EXauyE%2Fw8xmbwdqT9lH6BWks7cxIYSgP1fy%2Bp4TApS93NiCK%2F2P4NglzqlF%2B3pLRLgtYeOIDY7cy7J1VYe3G2V&X-Amz-Signature=e8669d678cbaf7ed8fd1d04dbd600ff6161e709f6cc5b417386bb8b7e750a099&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

