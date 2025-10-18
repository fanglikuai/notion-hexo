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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WA5YRE5A%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T080047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJHMEUCIDOtNVtRE5MsqNjr0zYFZUqAahyPvY2fszxZxLd4YVC2AiEAovJArzN3BvtIizu1tVUHdARcGun5Jpe1QKzTx2p1%2BmcqiAQIuP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFtOHz%2BkVjjfFZrjZircA%2FEqEgatkJZnYGZ%2FT7rOtKD%2F578M%2F1%2FQVULUC26gP1fUciBHuV3RL39%2F4diiFOewpv4FfIU%2F9%2B3TTVrFgUJY%2B%2BFhjoLF8hDdCsLvgPJzzGKtkdNjaMHS53hY8P4z5TGk8Fm1yt8AGyZQ6ebqX55I9r9FvMzY2NbRqSpAEGS8JgZHub8FOy9AWRNOCZd05BGcELXDNlMz7EjLReZ4esiUxhwXI%2Bx4c5EcrCL577OALCxNUMCsI1AWPCEltYbUxo09d9J%2BC0bkUSOnuMgWMKUKLzcecX47qo6cWIfkV32z%2BZpA93K4yHtuLWeJDhjKqGZVPGeDOC1n7J%2BgtpkhmSI6qlvq76q85EPc7ny4R8ePigp0aR6GESVWxUeLzhtEjP6UpmMj%2BaUCKn8q%2F%2FoVO73Cgu3%2FtqTMTP9SoUitADkGCjUy7HPRaNl7cqNxbvEuC8JZB0NDPaKMqA61%2BLEBfUXdO3Su3mNev1PLlTI9z6jM0MvR9QPhEeOymQgfAWPU3ZQa49eUahxHn9Q%2FqB68fmqBTgWP0LEHj%2FHMbbKELL0McU2f3qIJ8NS8aGYSQF3sqMHZW8P00qMHWSRp%2B0HcUfROwseSQ%2Fooi9f26jujpKuSgFugkcGsnSTxlrqS0IKeMMrlzMcGOqUBC%2F%2FlDAq1DezLmMfljhD4DE82lAp0HHykHqwFly1ZBI3Bwpx1ElO2UAzykd02%2FxsDcd9xrM0dKMlXxq%2Benf4ma1Pmn7xToicBlhyUCCUuV%2BWV9lv6KL0uclp9bSJQOtIDg2tMedNUwuw3RgDnmiD3GYfT4Y3t04yswzuw%2Bc9dk9v%2BOnq9Wn0IC037TA2967rAvh2O%2BH9Dt7LuvjOe4St76WBVhZMA&X-Amz-Signature=62724dbd0e8f15bc0dd9954a116901d5a94c77add2178fbc3119739aaa23628b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

