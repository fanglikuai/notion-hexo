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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T3P3N2YX%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T180038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJGMEQCIDFBxBnRBlIb%2Fa0dAjevNHImZQzGpzn7aF1385Sl8khlAiBNdFPQ25XIwtGi28YMUeAjEc6z3%2BdUY0xSznr722E6zSr%2FAwgWEAAaDDYzNzQyMzE4MzgwNSIM9gAn79qFL0RCcYvaKtwDuCoPM3uCD6fxQ1qM7oUOQCIdMfqNqzWKejvw6g%2B2gUd5k3t3SvMbZ9P4jr12rrMUQx%2Fp%2FVr8MLV%2FT7LEp35Fngs58Q7djSkpwcFbgvwFimzBpngvxlvKUFehNiA19q1VTstEGgdgyxLYh8GuiH2JwHXLiCMytKFiphlWnz1IOMAtXwknl%2FvCUzHokXRAXCXFBF%2B0IXmkB0yDqnVfOtVhgkNKMPQXExbREjlo5jnd6V4qxprzD0sqOjfF8vYO5uM%2Fbi2liAzP6o8JrkZ48m5lFcFj57uPalwyzSNWvxz2HtDAuMAadGT%2BZrWaebzzy3Y5XUOnDPCkvG%2FprjXdbi4fOJefGbx8Tiz3exsnJIPC%2FPhCqdTWSUvYk5DtYhKSKcsIvvmIK2gNTRtQk9p19raLCdn%2BnDkRLhr0PNalOoZkF4vgo7P9jmbn0xtAlGvf7D0%2B7oGg7jlZQYQeR2F%2FlDfi29YI7kZuoqa11UL4oyNClyBO%2B7wtGIkcDO7Ct7%2B3UBs8AbA65YlV%2BfhCZiLbOFyp%2Bfxmds8pL3I3YDQGIk8lFQkmXgq5VAlgBdstGydbJZCyrn3hQ%2BxZIQ23HdcP3xRHp2tvfvXZ7BejgJH%2FgVnKy9l6owODKczDU8XRedsw%2F6SpxwY6pgHq5mIes30FAuVkrQFnknBo8azKan4c85JwHsguCMeEQ2vOhrIrxp1Kju0%2FxhVlqat9OT9nYXX%2Fj60uTIaUwhll3bow1YiKmm%2F9gWa3apSyO2s02W0MZ6lR8soi3kSJddZkxBPo3KeorHVXJq4Zx1%2B1u04R22KqqCeLB9e23PU%2FFM18ZIJNc5mtfBgfB4DEBxI0TO740Lh4ZcN%2BfI4CKq10dNboO87T&X-Amz-Signature=43710545820d78fe0df27fdadf0069368d349a6a98406d8a00fbb514a2e44f7c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

