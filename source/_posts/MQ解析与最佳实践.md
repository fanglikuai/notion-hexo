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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YLHXQEZP%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T040041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHwaCXVzLXdlc3QtMiJHMEUCIBC4iDynz3tTV8HF3ySumZWDi48goJW8WVFTtZoz9tNvAiEA3FuFF4dUP%2F2yCSBJNmqdJpPDk9rVvBYmFh0b8Hlq2Ugq%2FwMIRBAAGgw2Mzc0MjMxODM4MDUiDGDa2TG4Dodi1qXiKyrcA1BXVwrBlZSSUnuk3JhOktqpNoxEKvzxwX4ZUGqId9InGQ74sG8aHJr362X6%2FurIWnXv4oSxU7IhyGS9EGRcDFgu0U%2BAVuSkB9Yr3z216OwQDBnvh%2F32DGb5l0zswDTLqGj4IP7KXIMtC5ikmc969gjPQzVKPAvUjeJ6hBl0ey5vyQV9HsgmZErVevQij0t%2F%2FTHOBn6XjLEVRJhxd31uTS4gTkZJuARqTnWQw1waJi4p5NWt7arwf1MBAjVH6VFvwPrFz954REZUowKkXaqzUcpbarptKGR%2Fl%2FVa5jLnnS0uDn4HoYp1IDXjPRRUyWIf9rY5RaVGvTQkntV77kLrlW6%2BaGKY7MKOOSbLq8wocIFRXBoCwJdGM3Vcju3Caq7OSvwi8%2FZHZvwPwsZgmCSl0pxQXa4S%2BmhM5QMsHgcY8LOrzZLDUZWhXGBkNHZAzPkc872GdYTBMxWQ4Onb7H%2F4x3hGANB15gPs7Y9rkl41YkNSFde9rEt2UZtVubP7bDluvL2OwMJDfK4aJLNqutcp0%2Fys1h%2FBI61ONk%2BfX3RUpeSzZXOaJLl7fhFUAQqvWIYaA%2Fx7mCn%2B%2Bn%2BXyGn0DqJA2bZj2kVl%2BvkAl%2FC35LdcpU4dNawD1vjEPl2NlsSqMJec1cgGOqUBZDkGzliIhxUWX8zYZ4bD7jNwglCriUpg%2BNKlEZUfURb9yb9zsnQp2q4OvGRlFzBz6VHwq%2BWiFGRGVNVvDxMddPl74sCY53OYB55ckqn7pPQnGdy4PTUqu4iWCV3%2FTsIpitph2%2BWykQkik%2FGHwXop8lBSQ%2BnBIO1%2B%2BXqzs1G5DGXnXFMNw40qNTMiPe7jLIhgAVs%2FwAUaWp%2FnC8gE4IeOjuRqdREC&X-Amz-Signature=e8cc3a1f3c90f7e7cc2dd7e17af8fc339ec533c7837f9b8131f87d294fe94397&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

