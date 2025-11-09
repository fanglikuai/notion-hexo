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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VNXKRXJY%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T180044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJHMEUCIQDJpcMH3I9EdyX0d2E8dPmK5FCbE8ZS8CmhVO3MEYyu1QIgBFA2fLorvOVqWodaBH0CwsS4mPZCJ9%2BxkapyzCIGIjgqiAQI8v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHNxOhuOHWCtrfSlWSrcAzpYjmvRCIzeQrH7BUReOzSZnbf8gdmbYCxf1ul6wq%2BmOSY%2FN4aFeCqBQP90MBIPPt%2BZ8V5NYSMtajOxq1McO4HiNROUeECFD%2BMjygtUksjEfa1rv%2BzHfrfKsCr3f%2FO0%2FRLhtk9vt9BrlZQ1eBpW8WBZVf0gYXIG1j4axRwcdlvzEEVBL5n0V5NsmCghThCHYuFf4HyOtkezsQ3xbuzC0LoRJ8vdScJusb5zc%2BaOezV8fvdhZzsB%2FtZH9BTqK0%2B3xlFYFX45W4nsVF2rmphjJldeOwSq67jB1kj0HAdnyuly98KHbs7iDHjKj0UNWew8XkREQ156xr2ObkgQk3L0lA%2FldIaz3vWxq0JChFUM6DqWu8FlI7Ub6Vb3yZBucPLUc5ijAoRI3ogJJkGrLiCfnZ9FaEEmCEflAP3%2FYxe%2BfQAs%2BAgg%2BKYpf4bQx4l0SqjH6cI7LnD1GaiQUfdAP8YgapawNo%2BZVbx8g2RziaTFf5Tmu7XFJIPsRVIf2bcVRftup4fPfUY%2F9yhz18oxHU%2FBPCv9KTa6u1XoS%2Foy0NtUy6RcaSdZPoePKOzurspSUCumzUv9fO31zxWwM7JV%2FbJNSoOqLhgWohlDmOVCrmuE5cLD452QgOIs7IZWWVgSMM%2BAw8gGOqUBU7fdCaUgm%2Fv4s4AVLd5iMIrghHf8xVjRfWl2g5pBB3dxYR8ByAIh9OzqXbQDJcv1bwZGyHVWM%2BqnBnHai4PczZQnPezjYtivLzfZ4W7tXPn6HXUBQYx4pFh6cYZAMwkAXOGsft5ImWYKAWg8lNmViuk61%2BQoCrWSd2EcStYKv2VtwaVr9iDBdjgt5KK%2F2p6w76%2Bf3Hp0m79E%2FjtJHQlCQNxny4xP&X-Amz-Signature=a1ab0b3c102706a298f51a937ca4ca30aa410d55bdea79d8a12eebcc172b3292&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

