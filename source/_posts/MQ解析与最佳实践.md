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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46656JPYED4%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T160038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBQaCXVzLXdlc3QtMiJHMEUCIG8wQLORvcPHJ4a8V8kHUFKGo0H4o0BjEAlW%2FA%2F6J5dPAiEArTSw%2F%2FjImlGfifxi53D68GYJ0q1XJO3NzR07Zg5N%2FJcqiAQIvf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDM98q93%2BUgevQ5ELpSrcA%2FpBTK1PJ6zoo9ybnnyL7jT5qwyNDKDAnEKVMLeVPOLwgYnjy1WBpLg%2BwKmpFPBj%2ByTd25OE6RtCtgRS0E7%2FPpyRWvB%2BFHhwL%2Bq8uhZGS%2FPR1oysEHoagPWnail33U56Jswx7NfDdKIUDuvoDvt%2BLQm1AuvYlfZNyocmglV0nsIyvxkEtGpg8jsia0T8u%2BsHwhisQH18t7k8Ai49BOdVni3J7Lgima3LXK4Oyrme2qKUFVUiIfusjG4eE0akJ2vDsGAsh0LAraEloWRUbhSc2iT%2FWaKmTLye48PHyqbl38AXIIDK4LFTzkCaz%2BiXn7JEPucaQy8qvV7JsgfVuzY%2Fz4g9ZkguD5B8RdkS73lB1YUDzFgPRlS8Wj2OxCTDd4okvhv6%2FXrWHDmC2Jmij4zF%2BcfcoCno%2BQN6xDH8YdNRla8d6kz5ajkvwz5i%2BU6uiQoH5Bm2YUyBZ9iBxRkrLkRvuHlJpGDTQD%2FZRza0A5FDXbInQs1Q9gKT1gnrM%2BtaO2VeUF5V4SHx23sG9W6dAY%2BbS%2F5mzsYKkx8ZTn2bgYMW1VlSyfNkH9n3HqsxOWF7L%2BBAyBEDIQ4txlxQ%2FM1rg%2FUOBKuRlQn9TI7rAYoIhfObbfXZTdDXd6zDyS%2BSoCY0MN%2BKzscGOqUBqRog8Xb8rQ9M3Wk5PqgwR0UDE0%2BDrFT%2FjSzT4msMsmVo962qKypTkgbs%2BDIJp6uZNIRadn8b4hMaKs0pFaHt34db0ufMpSO%2FSVNYeRNwKtoQ0ep7%2BZSd0BFZpre5tnWtY6ou27xzPMVBTlMVXg%2Fhh%2BPHSOPEGVQHcGHWeZ4woNTVpMXItxlEi09n8VfejJiWFpgTQnEYPrzJqOMqUf0oVHg%2FJ822&X-Amz-Signature=9ffd1cec6427e8554c15e6e02e13633fda899faad98c9297a682bed675288d34&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

