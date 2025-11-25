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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662A2PJABS%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T180046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC3xE9fXcRHM7AKjT7m4MdYiGBcdGlixq9FsCdTpuvF%2FwIgBj%2BZ59UqtQ8MYANSaKlwXp%2B7XIXClM%2Fu2HTfp0t6Vk0q%2FwMIcxAAGgw2Mzc0MjMxODM4MDUiDEtwnwJ%2FK4RGuSYNHyrcA1lCXQPuptKlS79EZqDoHjod0X04ktwGnRDsFq8pftXj4BuyrzXY9%2BHdXoW5pGaKktzRJCTHvDZtw4szX5izaRDktqUUNIiAAQr1DBeWZU76ue09UtERVSVEhW6b%2FafPbowCLEyh2x9qJMNMHoX0udiIlehafS6VAb5jOG5CfDmEhIvuFH2bN%2FpKbCerbady3GcGZOQ%2FYZErEJvFRTZ4UoreCdpXP672VZ50gWmhbE923xYIGNKWppvvXJ6bZvrl0TNx9KPVAi%2FvvQ0Zmv%2BBGf9TiU6Wzgm3o%2Fcv0IACAsFaIzEWH6vETZ4z6sKDDa2zzr4LPNiCiGWt7rhXHE%2F6ajFL2%2BJMugGZA6Qh9zDcyIlzsfw82heL4pASQKfAdw9gRXdXVqZrvRRGXZ85lPZ2XfqhpRIPFBsOMuVCexxTJ3hv1Pw4ZLe4cTEmeTIjF32tP0thLsPBXpng%2FOZ0WmJgmhrWpW1fYmO83Qgr8KAkxwpfYadYQEt4UTLovAfniANh7Rme%2FQCLGgwUadG5N1R5mvwzwQ4jpWb9nwKNrdh0FJK4BMSsmM88ZAKeMp1X5YnTEgJN%2Bl2%2BwjEpsol2Acqc4GrAFDMTyz22hyUMJVb5HWDKZpvSt%2BKlycqe9wkdMKXWl8kGOqUBlTZzGVa8tFPA3Q6rX1eWimTbwD8gLAFeqxbS2TeufNvO9lZIipVtlWkXiHQXI8quxPr2sHCjiSKXYYO7s1T1CSiGKhEUkulYBdP%2F33kMCaMT1XasMQCMkce%2FwJpdAvhb1e9WzQDl88B1STNwZCCOZTbO435E%2BM0z8V8jVtA5yzR7dE2hcjaDepXWCJQtBSH8UHlEayyENWbtH1MIGdTQ6dWGuPey&X-Amz-Signature=3976eb4e72f34e735d7fee86e06ec8b58bb5068ee47356f5f5bc69f942d33d66&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

