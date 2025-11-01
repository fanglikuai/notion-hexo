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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663BFEQ2PQ%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T110047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF8aCXVzLXdlc3QtMiJHMEUCIB37WkeYaHPBPUhWQnBxTDMdwA%2BSoxO1w6dxaU4LQ3vAAiEA9%2BrPWNFNbVTLNjZtbQUv6xtxsmVQwSVMrcVgYIe%2FLskq%2FwMIKBAAGgw2Mzc0MjMxODM4MDUiDEUrnEFEcWozRHEqgSrcAwdzKYAKgbR5wl2smgzf5FguRrj%2BlATXO3f8fak4aztmk0eVONgZiJgC2Ux7MzfFd%2FpMX%2BYEFp%2BtH9OJyU8IiTX%2BTmJIkkhh4hARwU%2F36tEs2drPNaqyAPqwGvAf3f2W%2B9yg0LKLuQYlDTJEPigW6hagRV%2B0JzMFLPk24T5h5cNx%2FAwg2yqKTcCNRw6XY7KjpwCIv7xAxLgpSEDhaYqUPDeVT0%2F8Uotn%2BeOupgT%2BMj%2FbPlDtB%2BUqS6iKigHg07vUD%2BHlAYWrCAvjMJpdCPl%2BY9UFE8UBQ4HQOSCUsfg3bHdO6P4St3dhwMdCMGvh1Fnz3i0jcAZtmTlpOJyAbAVccNP5bOJGAIMJsqzn74zTktPWMUwA2gdc3E0mI7nK5QCM%2BNm2YfT1etYTawYiUa52SsQfXvr5J%2BDIEVcTeDWu5SHoB78xSbm4ZI922s0oiFOoYLkq%2B5Omh%2FlWtQLPXO5%2B4eJbmkz0595XHAu5BPAkwrx%2BDcZ%2BihrFwsw%2BHpiiGAZuv6xnyJmDdQ49RK4tTlMllMYKU7S8mbuu8BPTWLMqzwGCiOMHYHCiYFbwWgQXafmWXA9OQI%2Frlz53TVq7DCjA%2B7Wy%2FQ57xLvDtEoVjWwo1PRCtiDCJGjws6JS6DurMJ3UlsgGOqUBTBszKC64kBL%2BgLWyR%2BoUtu%2Fm%2BxEZkUAeuXPNEfLsYZ8JysfyNOgXGQyPvwm0AcOMGSXydFl0MxuCNsCUCjQRgJ2FEx5REFq9oe%2B%2FPBWMGhLFuBJ3nwpDPxGrY65tXeqpH6fsI3jBBjK%2BfjtAlNUhbgynpxtcvFLi7uTXd1Bv5MvmJ5T6VqKd%2F2uhBZVAc0FX%2FK5DYGN0Bef6nxxLDGCLMQDyT8%2F%2B&X-Amz-Signature=c8ef41c3c12c74a50c20df9fcc2ccb2a998fd4dd3f5cedb92652abcef0737248&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

