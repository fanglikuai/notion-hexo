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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QQOP3FUD%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T000045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCOugAc%2BPVDtqtU4yF3mrq2z%2F%2BkiB1fY%2F19kMlGTGgM6gIgE0cJx0bq6LXtvLIYxw64xtvAQlSOWDF1fVWiYOclvnUq%2FwMIaBAAGgw2Mzc0MjMxODM4MDUiDA0BFuPaUWo%2BBTTE8ircAyWrIRB2QvwYqyJ1l3qZi4ENekcD7saIwFURewWKi7Nom%2Bo1tUA9syFUZhXps3qtuslWqKqHr5oqRX4tDnuwXveBQn3s8EiKNJUk%2BNJYomnLu2mA49BVYhzm9Fi2AGHAUOEEL%2FPSwY5ScK3jexVTPV98hlOhCFlRkyz%2Buoqht5MLH6rrUXgLAGxGlP8W%2FPEp2ubl7GHw0lAhi4doGxF1n38woSRLf%2BjXVSrOr8UVXcQWjUb60nDRuJ%2BILTB5aA67VgFa05Ztlrhz9x%2F78b4RM98jCJN43crE43YL5llkOqiB3K7F6jaqdD2QSWY43GH%2BWUbt9MVxkT%2Fun13nPraNjKCjHdtJQOse7mU92b1oNArWvUDF35TGM65EEDlFOSm%2Fk5FEcTeBYw8hTMvyu20ECDiaWACgz1%2FF0JZ8glMbTE6sFnuMhVquhDD%2FOJd74m7XQShfdh3eSQUF%2FP1xfxGlnSPJJCogbFKNWstaXCo1%2BfpQFNbTC54DjFsTWBjWjFZwfN5p1qOCe71EL2Nqb5uPDlkz9ESQulBwHPwSWMUGXX5piPel0fO31NU5oeDZs2DJZAdlHpxZspjYQi18xbCfFgahK8PqdOvsNwU5tKtRIF4edAYNuDgamqikKa6nMObo0cYGOqUB0iYhLKfMEGTd1mCTxPtE75rbfuETB523Q%2F9PoUfbbLY1C0jhJxC0RZQ0QzohqY9oYXaKiMZ3G3udR60JUkdPfxja9XbSlHI5Q8OBGJ2u9e397GzLlx0Vwp88BQcMTTSMcDwlYDT89llHY0tx6CaDDJR6xNHgRV3GPyzSJFyxIvZpNF5wSZ2T1vB39pvE2g3t%2Fg6ZWqnFFlCD1Ti922uEu4cr%2BM32&X-Amz-Signature=3aecf9b895aedfdf717f46f65f5a12c14ad63fd3cd74ebdf239a2600e3458ddb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

