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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S354QMPC%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T060059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDpPpGKEb4hcT1LMn%2Bz01rXuFcK7b1ib8aDVSJP1VrlFQIhAMpakNBm1VGiqBgH%2Fu2Z8bMwwf8MUjSxgdo%2FtJ2rseluKogECIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igyfl89CLXD8rvSAHQgq3AMBG76FF6Qu%2BJ09E9xK%2Bzv1dh2XDjTwnM55PBe3yLFiTiodQe2ekdrHL2cHSR196oHb4R3bqNeUN5zgdUe1LmD7%2BKw8j5wf31JU4X5CzzeDIeLUpsig9szH3EVgOWzZGhNkG2yd6%2FwQWcbjNMYGGTIRs9WBawq4fzqg96H8KxWBgY7LKK%2Fq4rY49o1kERMwNjaNIZCqaaejei%2FLaPf49OrDc8kyivGaiGfBYp%2B1KCph95o38eN3QtSK9FN9KaHblPOazPlOehMYgV96%2BNjHzxRp6MweQHeAGEle8KfJeqdCQUabLNLwuTpewagRwZaIBMy%2BjK%2BA9vp%2BBre7nkSRrORJLlZn2y6%2B1a7Qt18F2tWppqp5zboF5Vkooz0v2EgxY%2FabmdJS9RdCqpx8oubup1HhXooQvQflVAW1tP8vSVWPangLAPiN4rhasqyL4XmQvokPpEpTZTtCbUMdT6vC2P9cjM2xZ4AivvrSmY1nJRBooOOunwrWi3pxV0Grk4hIhO17GLRXtGtx3mE7%2Fk%2FctgeNirK0sODtDQxaDkz%2Fee9S3dpntPH37ViRw3IQo15b1fME%2BCzdvylMmyLiBWuA22XVgueHlD7PO2kxaVjfScbwHDCx1THGsO7tvfDjeDDX7%2FXHBjqkAYKbzm5c0g3BHkFz%2BzqagGFvn5VuJUcgqLMgeGSJbvkv2rnQNgKZXzRORlox%2F7GKxw2dSdjBTtP1WrDerMilUk2Zw3akYHj6p7AmCxdaLFWgUml2b6YeYX2w9PAyqWsQhZx%2BZJarqpwp5lnYulB6zPCsy%2Fk6lDHx9AFimR7S8MPY4o3tVoVz2EtOdiyemNuwg0gsLWLLckdUFT7gXX2yFNQOwPDA&X-Amz-Signature=d802e80c7fac420109e4dce457c9094d9c33054be82b915330ff2f7b24250373&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

