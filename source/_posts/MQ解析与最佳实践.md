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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662IA6XS5B%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T120053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDl9OVmrV8aUdN%2FKCQuqBfUA4M%2FpSqRTg9TN5VezADgcAIgHKJFiTP%2B%2FtctxoZmx30%2BWcQTHBzbvHwf3q%2FryzkOtdAq%2FwMIXRAAGgw2Mzc0MjMxODM4MDUiDNWUmBeESny1evIMtyrcA8MKxxd5sff0XZObdzt3sbzh16dhTbYMWrgV7sRJul%2F0YxVS5SlnghWWOY43rkEfoG%2FODPKrSUemjlC91BQGGwJ3gzvZVTi6df2alboc%2BqslE8mjz3jCh7rd83VVYGdvxzA8OpwVXPFeqa%2B57YuFMMf4KWyDlY3OwR%2BdQrpzLi3yaIqwEGQAb%2BsiwcPIUn9F4MJcDhJojh1d4lV8mf9Op%2FLjarl7ci6Q1%2FNlmkkF%2FLG4Pk57dL0JpI0n%2B0bb34jzxt1awkP5z3462naxjIsO%2BkGuPHg25dgvkKJfz6ZZz%2FOG5zK8h%2FlKXGhazSN3NPmtmMKOq%2FRzwTzBpbaqXLsoA8QVo0NeQl0PG%2BlwQoVO3JHQBwyy8aZk32koKzNI%2FphCjEA0U9aYqVasgB2n5X0mjuCHDOuXqoxNJRte5W4fr3aDpqov5XdO5uGV7m53GQwHL8QK6spxLXNxYHAfppX2MrtHGy49haB%2Flq908WvZfI08TwU8NrqvH7xPTXsz3g87LRYKtCiVIIylrP9mtKxsMzgdXvwgjOK2m2OT2EkyW7UkA0G5TmXF7u7JF9ihirm0%2Be4HzlI%2BvfvySJTQEGCAlN%2Fi8ieYBE70zL0Q7srdPFqwWdOXHt%2FkNkE6olp3MMfzuMcGOqUBVFMxPICSyyMzTdrWiHFTIV3zWhDIsWBZbe1qjn%2B5FTwivhFtWmE%2BlAtq6QpBri1nGftousA%2B9gsJSic49l7gGcdBpNs%2BaI7zPGlhLNzjGTNigMInQcIydfkzjQ2wZwCFHE1QoonSIWo4NouuIhEXnszP%2Bx4iPJ7FHF3Op9fuHuoGFuCbPZrks06wG7XH5lGrgpojmXD4EyAcKbxIaEZQjzr8zq%2F5&X-Amz-Signature=8a6d045ad24260677a6b1b70816461bdd122e646dd9734ee11d91489c9a5c2aa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

