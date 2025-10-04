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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R6KHIN7P%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T220038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDqugIpWJwb8OmuHLXLDXf9CwqX3GOmejYJP%2BUk1k2FJAiBkIMyDuYbJ1QxiV6LFKqA30Am7hDAlYCYLu2LrcXJ5hyr%2FAwhhEAAaDDYzNzQyMzE4MzgwNSIMtOmmmcdqbDCw%2BQrEKtwDL12RhnKlL0Zy%2B7urnktujJyidVei6W6VXZ7IVHq1klalseCQs2RGyI3qHnBxBqtJBHVtAoDUfVQWZq2GEB40O0z5frN%2Bsl9gEADVzXCjHsv4f8HSbWfBVoLZnOgBA%2BtiqTM18llIO5dCL7fUEMXTTStZMIbVuEaLZfx1UGcpwmIrmBSjVW91ztIu0VfL4A9gC4F3bZChewWcQfk8WLZp9uNmYvpvEOgH%2FGyutScYYsoh8k9KGC6El0LOxUV5OXLn1WGmasOOQI0JjQjm6CEmFBWa0OF4bRasdbixDclI6tnmgCcB611romnl1pg423N4bRxcnFhfKJ%2FLFzd7y8nQ54S%2FKY9%2FZzabAH%2FhEAwJH0jW4pPXftElFwhwLhmywetFcIHwxMgyY0565344ugs1ZFfby7zSAs09FOvjtmN5yDjUi9e%2BcHiilGFmHraiy24YNvTQcSp4H%2F7%2BRp91hJkR48%2BPbAkNqQMfdpRE4Pxp9m7DsPqdDiB9h%2FlTNM28wE09tIPh%2B1O1hV%2BNYdinELT%2B3ib%2FU1PCSphIZsZLvuejqPhjPAMJR%2FoeNfs54%2BAkRZDknUUHU1jH3wS%2Fl5BX6ZGZ6zjUwS%2B4G8mA%2B2RTC9nuIIlEKLYjybneKHn5tIowgpCFxwY6pgFMoCOgOXBUclaLg%2FqIzhRtxUtYUoh7npIUBgDiI%2BrQBULSnmZSObR%2B5w8ud0FKBx0Omzf3iPNxtDR3rF%2FDhtIGQHxdEB8Awhag8%2BjXxDNP3IdmWpCI8TDys19mBKla38%2FYEwKfhaGneJxaR3jAOKxvByfcbWKtQNlWiK%2B1p96c5xh2iQCyz0n9jar%2FnEkF%2FznOfrWLSC4n0ugx6yfgiS0IWGSv2iEw&X-Amz-Signature=9b4f31e485021b21fc98603d6712bcde5715c6841ecbc88f6e9ee775e26cf3b3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

