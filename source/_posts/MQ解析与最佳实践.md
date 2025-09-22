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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XFUTZONS%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T030044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCEM7TFJEc3xPydEo0u%2FmhhSewUt5ey7VZsXMuoMH6snQIgSNp7D5YMn8llp8ibsa%2Fuy%2FHeEKJOViovGTh8MNzHLqgq%2FwMIIhAAGgw2Mzc0MjMxODM4MDUiDF317Kcy2JSgFf3FNyrcA1GBaxGF7RRsC6oiQDofn08Xbrkac3LbDhEkgbw7R8%2B29buAY6jRGkorTXUIxDDouLt1RQmG77PGrI4raBXV9YTMPyRFp7FQEGNOBOF95hGOstuYmemctaZ0%2Fc7HyXlrDfiS5HuwUm23RrZ%2Bs5la0ecKK2Ha6FMJ3G%2Ba19o%2FqV1w3u9DTlhHbaSJcaZDBxa73QQAC4xlomx%2FkzbaZ0okOScW9cwsuOIO12ONMSvjji%2BtlLkwVXracBFiVK0zsAyg3nr55i5Mx5ENwg9XfnUHjqQi7MWxAe3X1Yk0t4J4LAONr7t0AYq9Gnl5YzKSV7SA4NSIohynl4RYCxudOMUyDWWHOawclGAi6l1u0KC%2F40ZQkM1AhYCxfcF82uPaYlweJigDNbikl%2FeZ7WeKvRbDclG9%2Fe0wrgMDGprHiTeKmLtb7FSEimEA15pIR4XJ1P5IjZHsMEhRCyrtkdhspgxL3PHKLMvZpMh8nGGnEncfCXIPg2C86h%2Fgm3SX0ORHujbjXy4aoPbsv0pFa1gfg6xtH%2FaCNkoHLiZHrIkxgQJVV%2FlidfUqKwCxrLke%2FFhJ%2FR3TJiA8B9Yp888ilVFxYx%2FXmStv8UYbvJnfngw2Kiju3uO8VjhAfQQ4J18ScTNnMMKwwsYGOqUBavTQU61VIyseqgFfBhwUhgAjv%2FS%2Byv%2BFlGSED2SvuP8QSrSGGNWCAVfy%2FENsCmooN4NTf1BLc4IPalWR9loNi%2BWFDMSP4%2F9lQZspP5zmMCbf1z%2FULLHdVdU1PFwDpMy8pNsHthmJtUtAAKZ25wfmNWo7RrUfMigftT0e1NgD8OQmBzxvXWijZiRcSD7RKpxFxbfTEiVHczxrylJqv9huHjhdhOcK&X-Amz-Signature=6964e863eb77a73d47eb84687cd70c194f69a7708b960282b5d24ab1ad30043d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

