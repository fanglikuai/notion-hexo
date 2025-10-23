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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YVG6NOS6%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T090049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIH5x9DC9KYfy1s5S5NffZD3ktlY60avi%2BAGl9xQ21B7XAiEAuuCMB5XR7nFg%2Fuf5yjOlZLNfBz0TPghpe24klcRL3RAq%2FwMIQhAAGgw2Mzc0MjMxODM4MDUiDNeeroBCswTl9qyPUCrcA0B0xuubHmqXEyGQPV1U9m4lDgHEpUpdUdqw%2Fuh9wKa2mDBXbyFPICWEeidvmviASXG4OfQ5BeTtLp2DHBUIarp24z2fBhDMc5rT7Vz6MOfSRyZ%2FutIyIqAtSk4%2FB9oPjjv3E0GpKMf%2Frua6I2%2B3qcTlsdY0O%2BxYF%2BKkBAtyQ23SQiTMnsj6ty9aGqZOhkmYuaUXYcZZAwOJTMDlRtb3Q3XynNe51Ni8rGJ92pORKnGxCIufs8wlOtkhYR6Hatiw93BYf31wtq205MNlPXTwwLEg%2BFgLnB6Vvw2cUoqkqdwQt2dDf1fwRrZ9N49kxvnumoULLp2AU6iPaAC3UJOKhqJz77lYeZTXfmEE31QL9%2ByBwqy978xdPz%2FENBqZ6Pq5tRGCY%2B0sNwdMczqC%2B3zctCuT1N%2FlPb3LNMRxIAzMUMaqafoygYVziezYa4FenJGi4%2B3UXK%2BcJuJuU8TDRYBsDA2IqXKJLRxnmQkPsam1SIpFIa3ZQt27uCMjVx5PqtSbr93W56Kc4wQ%2BFcu6uTUQYguG12vc2UwaNBjUKnHGXvK9OXPQMo2snolYxdStpTW3NzMHprMynLVRCtOzJZNNYNJwYKjrWJESF0hhCftQAvNRO3s1Hzzw3Eq053J%2FMOLR58cGOqUBrHsWy3nRYEsJqkTcB%2Fr4RuGy1yKlOBig2vEGERdWnUXYUsAgXRQSw%2FrVexB31EUIa0uksMznsYihK6iUHS9o0vgb7uIZk6DwSkpdCby1YexTKi2zjPOuVS2B2x4Ob5BMbeNU%2BWjlYWUytACzXN2oY4lc6HNSLd4a%2BtrxOJNImYeM2UTb94qVRjxLZ5117kmuk3Hi5gyvhZueGn5MBeiH%2B3Q14ZEK&X-Amz-Signature=19e6c97992f3894243d63fca222bc63060d4c6b529da9d5c228efebbbdcc44f5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

