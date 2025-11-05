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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SEWJJUSI%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T030040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDT4aqlwVN8FRGH2rWYRJ%2Bwc6doyWQR6YqkMJZ16Gc6AAiB6HIA9Cyg6nxEd5XNijT%2BvjF8gtJHe7UUPqflWr0ne9CqIBAiE%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM0cL9K%2BGltt2dx2BgKtwDOjtpbITm%2FGXpxWHirXiyGwty1Qf26Vl8GXfBhdsHAs%2FHl0VWhEsFoRETasllDZRSGFCDy82r3Tz8C0%2Bahm8fO9RjDCJlCtte%2FzKCIGzd4Ml7cZx85sY9p7Go7s1B1rsBI82juAWZsOFXhY52kfM9ntyEqtc9I39PscHK2MdCvHStgtEC1xw3HX8l2zxi6GuxL1gOsQWSemFUoDMSHgsR6K%2BwIsX9tgZ1Lyha2Ve0uAn8PVwKxuwpjscrD4E5BTsllQ0CJPpgrTUpjjlIVJ5ZhMDb6cjsxjV0ODo0wCIeOcbxH5YngYmV3OtcCX1AiiyIhf5huQrwHLPc3SxwDAMW88v7wi9M6t5BBbVfoLdYabdsyow8JB3O7icC05SCOdEBRZCSKdULodOcnkuBPnKP5%2BwgJQRUxPvoaa7%2FjBJRiXKED2XW%2B4koIFTfbtkttV%2FvmIEiMyYk9mAN93FsECQX01ujN1h7qxcxNdN5f0Avv7A%2BnnKriRC1oSP%2BWQF5qXw6vrXfJeiLUHzTotop2deH0j2pyY9uBEyDCKu4wqryQoHDRMW95uF%2FhHWe6beOiaiH3H%2FIRCusQAYxPEWBGXVuLpmgn5B7mmnbZNKsBUx3xhOe9jjRlUfcE9PsWBgwkfmqyAY6pgHeuTYfiRXtjfrBIP44cq%2BD%2B81%2BG80y8De3VEqbkw5W7VzMic31JwV1giF4MkNhXPDHyZLqJgDInxZaM93PwV83irUenQ%2Fa7MmcrTOpy2411LYXf1aFEbpfJI%2Fo6dJdYmDgWbOMlyZNyTCGKQNPZRrqe1pcOqha0Y0ectb7Lu2FGJ8%2BP0EeNJqfFpMzMqRMlMHXzNHPbBISFfpSJdSEh9RwftfdeVSO&X-Amz-Signature=3e560fd0b2fc61ea781c85f47e0b95f8b0d435ce5078696c29af1a1dcc6cb99d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

