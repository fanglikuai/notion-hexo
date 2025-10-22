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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46653EQ4NUI%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T170055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHkaCXVzLXdlc3QtMiJHMEUCIDNCl1cgBQcczrZsQ%2F1D3MOrBSYMGQDjVV5OK4lnsNqXAiEA5iwqpyBmLvPaQSHjqmZrlvI6VJU9dxv%2B%2Fo9wtwZheOwq%2FwMIMhAAGgw2Mzc0MjMxODM4MDUiDAcaMlIUc2oVeMBInCrcAy9uZKDqCpfRgaeVTBybyXnD0DxwKZPphMZ90ATO96bf0wh1LOkaTlIDPXTMsJb62k5qEna64VdUq4uUBODA%2FW2Uhu310%2FjhL5o7OrhF%2BeSDSZsxUTAbKPskJze5QdSeVprhINFa8zyoJ40ZsSSE0dNcpx84WVNmmjuMxaPuZq9iUJzqqePr539UuPOk6uCGEOsY6ZuwY2JYhE26RrQ2xSWp%2FIpAgA9QA%2FPwebHIrL%2BDiiHFMSmOXkfh%2F18ZsRKBMWbyKalOS2Xki%2FVZ2YNwR%2BHCa8nRrOdFWAaPi1gpDQyXf9V6boVWN%2FSZZcFZv3IcPr1g4zng3W3XDrKUB972zHWV85D5Gfdi9yLvJPEBkdFmM5hH0%2Bkvq6SYx8oA8p5amWLXXbEPWKirWYj63DseMOnHOMEVTW%2Bv4PDYvp0eC1xugUSy5QZYzC8iyEoGKBKaVv5jhXSBwgk6lsLEGpAYuHqX2nCeZ7P4I128BuhB7vm3w0nsa39QntPgVyDuKJTpmhNq%2BrbWGNqCGy9qeZzJk0qTSSopVzAHfJ35QKPU4IEyvXkaCT1j9m6jbS9z3BsMcJcQHKstV71ZtDea3dDp5HIH6nzlWmcygwOODOD1WKvmDVQnq8wBUFW1K730MM%2BW5McGOqUBxKe5WcBjmNMf6EZXHHsjxJhb8bnF24lyI7mMKb7MJzkF5Y0SvDJ5a8FZkXZffvouqimGzpt925%2BbwWzlayI75g2m5GZYQebpCHI9EmVpzn0IKGW1Rv%2Be49G%2FJgYBg6JgEPw01fIPnBh%2BH4%2FNFXBWjxotrlsGz2Y3dak7B1KEm1K7AFJBujLCYawIdHDhUOGiYM3lTvqA3IGzbjlkByv1hECJCjMM&X-Amz-Signature=bcdd4f00d308016be9dccc834b6559d61f03d8490b3b1b1a20b9885a8715d29a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

