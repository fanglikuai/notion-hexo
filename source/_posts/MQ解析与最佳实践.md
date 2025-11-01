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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YOZXYIU7%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T050050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFwaCXVzLXdlc3QtMiJHMEUCIGob7UOxNnKrb4HfHlaSooaYN50QWqg8fHFzpyySRdUTAiEAnvZQxOZnrjWfK%2BMB7m2%2BC173YEk7uHw0M2Jqv26n36Iq%2FwMIJRAAGgw2Mzc0MjMxODM4MDUiDAWZObrWXap5AhKnJircA6W1O7DFpEDGDXIvXpdv%2BTd%2FLIU3GAptNmXzQRqzASxRM3smByOMkbZVOJ%2BZdvC0SY7HxY5MTVt6c6JoVLVPxYRpR1nQcY9P8XeZwt8qB5fDZDXrAPDDCMpMQbElL%2BnDmgIuLqFpoRcUf6USymDpzAZUKqMFXhzIRbVFrKdUJhGln%2FZXY0jD%2FyWz5YE6qqWJC6I1s230EW6dzPSNnArkdIVz3IhxS6HneXyw0fqwTJsoF6P%2FUO0qP8smX8HOkvHvY5er2qheLQsa%2FyUdTEq%2FsVUv5xgJ%2B4ReggeKGOenfhkmxjk379QXtaQV1UJCltXyOw%2FxKOl2%2FP%2Bnw3rDFAAp62%2BNwqLr0DiPNdjBZ%2BBbYsv82RQRIyaAOUMEErId9BzLqyswuTVbVKNPB1PbQH%2Bfr0i76BdIJdNBHcSqi5gO5%2BoVkVaM773EGVgbD9Gnz%2FifuBPGgaKAr3Azx5lGsv2QTKzWkiPXCU8QrVLiMZHjII%2BygTfsgkad5Sh%2BIDsrJY%2FhfO7SFO9l5pZramtow5GvGXI0GuXtUIDPrZmRkByGKflg4ZddO1xnitHwEQCCbvGqRa1WVQxickHQRvcCJ4V%2BkcbDgdJnVmvS8c1jI4vs2wKW2zfHV%2F4xi7BeBZDdMOqLlsgGOqUBXvu6dVcR%2FTU2CSq%2Bsj3itPSS85bjnaW80hMRbwH%2BUuHWWU3MB7P%2FuEcMVUbrmvKesjcQxrMc3qP7uk7J43%2B5%2BBqJSMm520xh64qfhKbck1aG6C4n%2BCFTlSyguGeNrif1vNoma3u%2Ff8IHGMRCahDJrT6l9cO2UGWK2in5QR4qyUMEkijzt3tStiNQOvd4A69YGL6VxQsyslbupbkg35YAcUmXK1DN&X-Amz-Signature=41ab4a3b4bfa4c5cbc13676ec7f4159cfc6e7fd29c84ededaa70005af09a0ad3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

