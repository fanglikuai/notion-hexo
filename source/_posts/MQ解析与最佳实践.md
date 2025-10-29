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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662BRCF3OZ%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T210038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECIaCXVzLXdlc3QtMiJGMEQCICe0Q%2BvunMWMN9L19MZn9kAlFZzNKRzEZ9R3zAiZ5lsgAiBWyeA1zEeQcK%2B5BdYjCPBNJmRSU89nZxqc8S08WhogZCqIBAjb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM9eawOO4jKwSxb2JQKtwD6dhK4IIdI7ODs08GPxlP04M3D9uBSysrZBc41ktpbHoAmAn0Pf490hd3MoO30Oc2JHl%2F8ZjU1HYp2LqCy2aPwXlUPWZN3bg90hn0xsTLBdLTc6UjETJl7cA25Dut1WT3H97%2B%2BuYHTrfKXYckLtvP%2FdyNpV5YjmLgfoto%2Bob8FvMaOMffG9AC5Ceefz11eJ4Kx%2F1TharPOYDmAV3Dt4TPV2hRvoDtLvd0WUE%2Fnaw95YdeB1aoDw8P7rd8Boa%2Bpua65MP1SYrsXRUd6eV8aKrxt4qN75S6kS3%2BPkMD6vh%2Bvrt6Vkw5clw8Zd5kIrPThxMx5DrQOyZe0DSmfXWm55rxjlx1NK4u6w8Fj3hOL4UomK3JEjbcegVLgOBuJBx58CD2uuRJsW0RgLBjFDScMLzuhcL6BegZ5hmWpFwQpYMiWnPCBEm2K%2BwKFpFgdWs6HuW3T6yRKOpspmeKvlHU15SW6hyUrkucietz%2FwwaeHvMx08QNStNXd17wRJzUZFgix6OmA7wEqQ%2Fna018BxcdCi3O4yqHLnqv8Q8qvi%2FbtlOpMMOjFz%2B%2F2oZaOoeXF341yetvvJNyDTUhOp58IkZeMvmSa693xlIA%2B9AE5CJ7C8tHr%2BUnMIzWCieJISPPo8w5JuJyAY6pgEh81cE6bhX1QgKaCQZljVm%2BHFZNjcvoM%2B2WQ%2BNUi1OesTN067MthV2R8OtpsjftC7LJV1q7dTkltzSvlvWEXfz%2FQvu9XGC3bCGTDvNBliXVIWUr0xqUsu6kIS%2FmYsbMr2G4ppRHPubHz7LduXiWBSwLC5l7fpCwo8TKkbktzuduSKHLnP3pm6%2FwQlI0SwixM0fE7hQUBFGZxwrB4wpgquYpF%2F5yBmt&X-Amz-Signature=ced67a924c7447985fe7d438e72e9df5facff2016f7b2fec8653487982a16691&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

