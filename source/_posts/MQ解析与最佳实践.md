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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YVXRJI4Q%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T080255Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDWKxjJ3DKfjV44mG1cMeIZDLAnxzGwz2F8BXUwQZ1EkgIgF0SzDE%2BATVu8E5K0ak1RaJe6rIwsdsyMVrGviI7Ccv8q%2FwMIWBAAGgw2Mzc0MjMxODM4MDUiDGFH%2B%2Bb8maMhvSavdyrcA9WAVsfaW2BEXU51g3E0Uk8G5gVnJvephcQZMG0E8rWSVgd3CRgPJwg5GBcnNSdWdosTXxFkXNSKighkbGQ%2BYvfDOEmUbqvdguIS8aA7W0BXSFWEoQZmZdasWi7V0pff7uqnD1qONMyRRHNFpjfU1FgfPF7sPkoXGHgQQB6HXYxmReJh1sWV70o6W1gfLOfK7cxvv%2FIlR2DVMJyFThUfVeRGivQooG98xlJVBzz0Aw2vXmKDdDaei%2Bzd70qJhgSBAdWX1bJ5km9E3wkdAZZX2F42f9KNZBGtVCYZiOHtCav3IzTp4Drf5FPGSfY8bsf%2BT2s7ZvoLMB%2FZNpBLGG40w6vBu8W8A4hUADOHcwKNeyKFkYrytxT0ogcZdDmwZt%2FFTJX%2Bcm0GzEiynOw8GHZdGAE39q7xmzP0HQOdX1bLLUv%2F3F5QIG7gTbCHri0A7HyjqC6ELo7rJuR7%2FuDru1Ur%2FMoA%2Fg5zF7Xg4AnyGyaYxBee%2B2APmt8lBrJhWCGBYxvkSyRwwKtfyaf2LfQsXGiGRf%2BOwmY4xDir9cYKxyXk4x%2BK%2FhESd9lpDFfM8H5waUNjJnsz0pAddme93esHwMtexVf5yertMvHQ2trtxVRj5KA2kK5s0jboqGtWQ9KLMIzP7McGOqUBd2khd38P8PLfqvtE7zGiuoNRil1R8grx%2FApjg%2FhSndsaz12AmkAZzQF3nLQ5gJ4sYEWXNvtY655GznH7dqfbcdkWSvJyPrE7ev3ICGef2J4f5cQn%2FHSTqgNCFj6jfFEq6GpUV%2Ba1QDXMkCaLTb8ilzNOzvfV1KvP9otX7UrJ3H%2FCpgJE2tXIISqgerg5JQPRpfaVB9KRihJjLszSY05fKlGL%2FuI6&X-Amz-Signature=9912fdae62da9fe374d9ae1dba4e15b01dc599f5f29b5c66027ce30c53be250f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

