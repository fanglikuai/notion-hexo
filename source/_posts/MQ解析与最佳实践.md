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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VX65JKQO%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T130102Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJIMEYCIQDdxRbnQtkoaZcvfcHh9eAkvPoIpMT520z3eiURTa16wwIhANeacX2S%2BHfX5rA9XHy9X9jlojUj%2FurRIjV4MVFzB6g3Kv8DCBYQABoMNjM3NDIzMTgzODA1Igx%2B%2BJI9ZhrvRspRKCQq3AMUenTY5SjHgXwQywvxZqUZwNKo3iBTZZSAOPHBdlrQ4tzJoo8QgpFTSg5%2BdCXV9sp1VlAT0o1UhMlhwhzENWadzilpqKSunb6MYaPaZNWXVxzd%2BsoMKMu%2FtLc%2BPvfRhlNFMJHJgHHF5S%2BZ3OKb1SPiaLAyMSwhQepDdaVh94HrNiUy41pqp4hk%2BjBtfOSI%2BF0024cTMNiezWpJppItVWMpolJoabB2Ckn%2FYyS%2F6to2vDRuJR%2Fr9hEDRa%2FpyY3pfkwkycZWdDLGf5hH8Rs5GSyqXEJAqFZPE4SNzCReZ29LLsezMjD3myFM%2FdGXEvGG9D5JhmWbS5yOLOG47BzGtXuDd38J9u6N4WmtjgWbhOAg4EWF0LDKRvjOesaFYOwhZJXPiIyLZvi2VRB6jWzLCDMpnQlutKT1WSzzVuHs2MycufLyJSFpeH6msWpskm3Xw7g8zF0XbSDipCZgsLXD8vyjX0eX846HhRKxPx1Wi49dOGtP8p4mVHdCyHlP1TMWtt34IlSLJlnoz1eSIv9w9RoS843FyaLLQHhi0cpbKu5ZQadoHdhbiPIM5tqd0PIJ46nNX1YPDiSsvs%2BS0IU7mfBanwCikpeL0tZP0qZG5LVvduCtYGIJGsTXqBqkMTD%2BpKnHBjqkASch8kNwPaRPiTZ1tVxQD8ED120brzs5GkKAGkuapYXJZ700GJaR6YxsrMycqh950kygg428cFn8WP8DtRM6cQKrrl%2F6KeUY4HpFAeigbIrilwua3VsE5E6CK24rzZDVvPnXkZcAW6kmBv9iolZp55KtM3MCA%2FQlVsn1IjUtUKMTON02uAhSISxwulh%2BIxlmd8jcoDuiO3AHAYhQgcvRFjaHpg5T&X-Amz-Signature=8a2dede9f06a1a30421a6f6c3a4c95bc6fb9ad8603b1aa8ffa3f9752c1a6b454&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

