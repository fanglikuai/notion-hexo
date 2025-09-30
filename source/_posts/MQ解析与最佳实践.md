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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WHTDOINK%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T110043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGMaCXVzLXdlc3QtMiJHMEUCIQC83WoyLZuTrMw%2BzdyX2o9J0vI%2Bi7nPPblGZZ8n1miSZgIgPnY6pwxoRynSCH%2FF4%2BBresUZk6i0mmKJfn7YtXw47VoqiAQI7P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLeEG7yjkfVWDm5R3yrcA5KwFAH83j8PYjh%2Fk53zKIfKfh2b65BVLQZqRI%2BEc77modtwZxU4Rv004bx3paaU0YuK7jskUo1HWd%2FjwxXS%2F07pk3i%2Bgc9asdvkzG44wlh220AON3UI8eRs64lXsakucv%2FtG7uPVozc3QGveWtJxEwPPuQHDJs7hYcer9SnE6xXL8bVR%2Bl523J%2B2ViTlxUNccwOc4Kly8756D9cQvgkwICdMjK2emUsN%2B2mc2%2BkeQ7Jyf8oHu48Ie3ruIFDXw5CVWZ6OF5oCf1WPDE%2FLbdKml9I6qmGeHGBstridf2Svw7rYOWcRBjrVabkHWuM%2BJe5mAPa5wy%2FGJo8DaxIMD6RXSGH8gdjSQaxBhHUKU1HqIrLmhqigY%2Fe9wnjd0e5y011bZRGaqYHmFKYfwz6EM%2Bm3Lc7Cx0RvXbMQVnzLQMsY2f6yT4Aosbg%2BWWC%2B%2BxasHVpVeWt5vVRzBP%2FdXhwyrLEASnrEYYl9j73mkp6SyHTu9WeP8ia8Eu0KzJhqwnoWYkmSw6QKlNerBbAqVZEh8xv60S9ESEpN6BUJ1kHSDndmUAx0J45Y4ISwIqx9kC%2B8gWkU8B8WMxxNRU1U0feJaetRx3RwnnjYZYhgbXyk7kDRmKw4dENZHFopMKUWQE%2BMPzp7sYGOqUBGCJUsKiuwyg%2FlHJ1P8WS1dRoQqgeQ3exrZXshXAYYCm0Kv3sOba09Pw0ln1rIqdCq6QExedsvnJ8JPmzdzliUEbYXYjCwooMsSgBNKcGnwH8qXL1WFg98UnnanQ7voS6RFbhwXLAF3dTFMJaWVZ5MGIOcRtV8i5GEL4kIh1IHL8BoYhjijxm%2FWU25F%2FcgN3%2BGXzJuKxAWhfDHx3VmFROlI5Dexji&X-Amz-Signature=b722309256faeaec2c5594aa389bee2ee523f759512476f930a61516cb04e44a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

