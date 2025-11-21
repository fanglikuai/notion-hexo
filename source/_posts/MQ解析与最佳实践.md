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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46662IMYL2T%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T170043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEkaCXVzLXdlc3QtMiJIMEYCIQCudVeQMEXBTwnivbZVonRKs5YO2EfqAlaI4cf9ATJPUwIhAMi8%2Fdt3eO6%2Bqy0VyDQnm39cIX57n7qRR0X%2FsdVwHKPBKv8DCBIQABoMNjM3NDIzMTgzODA1IgxmlBs8rieNMFcSVmUq3AO8dXbmc220AFgTUmdc5GykY1MtCrjo1p0Q0FEtOnJ22HkXiiBt9MQGrV%2FFt9%2BHrRCuVEaUw7af0tYOrgNlOsT%2Famc4fe8pq6%2FKr0n2yA0kJAoYN7%2BMiNFbd99umLag9bIkbYc%2FxHBNqSm%2FxjVf846s8JsX4j%2BRaozBK%2BWgsxl2gViMNVNGlKqnnsCPSM9FlwVXzejYe%2BnrlYBzWJB%2FY8jkyyUOOKpgUtw8Z9T%2BMs2Mkj8F7f%2FRUtyK52TKTGhgTwWEzr427CnZsgs6k6jFODHjMMmPpKQdJYJA%2BmCiS%2B7mvPXpe9epLrR2ihIMMzimBA9eJZA8eWInUfpUen9tgwimw2jFbwllXj9HYFpeKaXCGamCvDL8nZTYhfAY5%2FUcow7hXukpo93SIxMvosjPLwCU9UN35a8qthyQKKnwuTeJepHWJgyeqgI23OagXFj3qkEpaydRj%2BaahE7J8N%2FZpF7w95WJ3Gm0HapvmAtzjtYqBN6i12WhpttpnXuDzED%2Ft8FtYuDUo5hY%2BKjcmS%2FGs%2BjJX%2FpC6Q%2Fu3xOW%2Fy0pIFcG9yoQCMXC5GaIzRCzzK6td9d1NhKUAIszEpmEMP3xNnWq8yv1npcZXAfnpfqqrGGPF5qJxZkfbh%2BWDo8WGTCtrILJBjqkAeNPIBb1tGKZD8H23%2B%2BQtcRGAibBSnMVaQXjbmHnFQ2KJRpxtktpXSSm2M7ewf7oXzOFphGtmhd6dtng%2BOQakVI074WIyJDwCPHVh3bgeAOfm8TDsbV2j36UiSezqGmYPq%2FNd0bZFSo0QnBw5xNv9qT6KyVprhIgi53d01ko%2FuZf9UEpGyfOF3MdJ%2Fp0mraUPSopZ7OYIwfI%2FlZeR2tG%2FwXWXyJ0&X-Amz-Signature=a4437cd5267c7263c43c222bb9144b7689e0dde6b1efda07eadcffbb76e27fb7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

