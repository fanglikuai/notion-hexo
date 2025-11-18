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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664K2ZFEN2%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T180039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAEaCXVzLXdlc3QtMiJHMEUCIH7vDA%2BAfpg%2FOYsuI2IZmH7e6l%2BveKG0BE7Z6DbmtcTpAiEAgJ32s%2FICRf%2BgYVqFNSGdDPKZz%2Fyqm3N%2Bz1fij7Ld4ZcqiAQIyv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKFXneFuaYMYmwqmtyrcAyQab8Lo3ma%2FdKZj6BoMk7y5D9Iz9xk4GrlSV32SPPZqxEPdqTPZhxJm847pjSkbO%2Br5fd9puL8My67uouE9WztWlFl1YIpqk%2BEvv7vsTNLrt%2F%2BGtv8AJUWT%2BGUVyVq%2FRiPh%2Ba9Kkw8qd7vVQ%2B3mePE42so0daVJYl%2B1KXne2tsKJkLSpekItJATQOUMHT4wqgJovfAu2UC1TdsWzCOUu7MMn%2Bf8OthvVimc6KjxcK0gM6o0b53GreiKbfcHaBzGaF1eOdLVxQrLQ9hsST%2B16cBPQB54rN3Rl1Jhvast0fpa0c%2F0wxmGs2HtUWHWZaLnSd%2B0zQzKRSPGEXdHqdxduj%2BQz8FSBYLPlqdk4Eq9oiChS%2F6WG4Dt%2BCQWmPF0obCKgJE5S1ZfWh7fNar1qjdortxh%2FVbmPKmlPQWOBifRbm18QbWtps0qGXFlODL3ObWEA0a3MTE7q2kqro3OwD%2FRSbfWXoyx%2Buc%2BSI2MM%2FHqJmXHwzod%2FFYvh244oLd25%2FUEJQnTuqhE37vrYTAG1zH9DoDb1rDudfW2mPTRIekFhxLBNhZ%2FMsKiSOJGTzsqKr6PcJD2ZNhxVsebrcUjP0q1zJ8HbKyBH3RmXcVGqwaUMzkC6%2BllDO7Xmgy0hlUpMIjH8sgGOqUBE2CC1c3efXsdZvoJdvE33rrDQW3ycXX2U5pt4oWHKEFEo8EjQqm%2Fv1T1JnyGu3I02cdWqek0LzZOUBZTr1Sge%2B0jT8e0jAPsuEBSzTRP4mCnPCj9oR1xaRC7UdOusPu%2BcX0etEW64tMo5PHI1ZQ1L%2BWfxygFhDsysgTkWCm%2FyzM4IKi26Y78GlUVxmBTHKsGLoR80cxq%2Bd5omp13YBUKS8Bw0YpW&X-Amz-Signature=c2cd9b3f98bdcb90f4c1c006a270114fdb7635118e65b3da0341b7878ff8ce6f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

