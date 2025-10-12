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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664GTAJSND%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T130056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIELxnRB%2FFxjArzTwDaVx5UXh8gfKOez4x4GG3Yr9S1R7AiA8WZhyOvh8eJJSKxty7JcC%2FnB2R%2FtD%2FTPJH3B0iBH%2FaCr%2FAwgtEAAaDDYzNzQyMzE4MzgwNSIMaQy2WwzCnvI96nFRKtwDhoNnyPrv%2BjbSsr0kN3kVR0Sd%2Buo9nwRtrLWtQlGt7f3nzQVqJonKUozrVCvfnj0BOTUtVGEb1FvQMkYt%2BvtBI4kN0IVmkUkVbI%2BqFEoQJ%2BvFZsmPe1pdzpcO4fXcpmmwE0g15byK1%2BfCMN%2FY%2BpaTYHOQ4e6YfsaYOxlRZAgUotAwqUfyKE5on7XMqGhIB65RCf7dI7GuVIcOQm%2BLYv%2BYXSk5rVMCtULJPaghKwjQRg3ZZljuygUB0Z8hnjI3BaYa%2FPcCE2GNTYMkIBCro6Esl5ZXoH4kzZ0LXl3Uv8HgJDvKVR7OP853LFXywPtXBtY3frs%2BobBxkwcWadIwJ49uCib4ug2csGfjr7iCO%2F5wj2wagnjD%2BAokun%2B77yETXzvILpJprOeMiRRGikNPzUirIlzvpuDatXKV8Ed%2FfNmfb0M56ntXzLcockWc1xmRDipOWdv5XwdRAORKj9z40SM6fYqY4PMmV6vwA0naZeMpRudOomUdqYrsYAhYcAWxf4J%2FNgYsxsO2VcDljzTLQ%2FYIg4rYy%2BzVACqOh3hciwVwnJ55cLB%2FMzR7Nf3V9FfiezCueI3Xd9rOxf504cVy7A999%2F95v3kKi16HIXDofYE35Hwh%2FDFs%2Fpdwa85Pg8Awh7iuxwY6pgFAnL%2BK%2FXg5%2BsJnGGq63VPJo7k2qT1vr19DfA2AUAMBlQKTRAEO2sdBkL%2BVgr%2B%2ByUyzLtXLzd6Q5VEgw%2F0kuTv86%2FJbO7u879ilY6Jfxkv5AbaLX619QM38LUFLQj%2B4UpcEBXndm7uibYPg2tQegRKJqaDeJizP7GK%2BZL32CmsFAwZHupy3O1EGBDVi61DEdKtCL3u73LV2SxwOwGrbqUjeHCx6UAVC&X-Amz-Signature=8d8c3969c9a8431613c4c9e05ffbe38902c3dd28a468edb2275d7c442dfa2978&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

