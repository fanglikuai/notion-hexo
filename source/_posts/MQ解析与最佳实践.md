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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U763VWBO%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T150048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFL3BlfR91gZrkq7ldPxKlZ5N68grefRQ1U8KvtrSlwyAiEAjpLdrtLdAgMjn5ZOGlQnTk28x2UhQimZGZCetZGuAGMqiAQIiP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDI9z5XXu2DhhS3mS%2FircA%2BzvZCWOYkrhzUhA27%2BacHH04aSRVfqpsxbIkpLa8Y8BO1iGwzcAz4Dwo9DNUxQRKjGecrUvr7YbrwBbEI57Va8Uw21ETOw76C3BF3d9%2BR2TtfCfu1ch12GpDR1ZZbUzgqM%2BWrqOzpX5cGoqJRV6FsT%2BXZun2uMbME2NPa%2BZ0xl47hyarb8DK7FmTVkslkBmUE8lAYMUH9ElG%2Biss9UAUjF2NH%2FDDrxFH%2BBwqNLQh%2Bv9pvhj%2Fmn204d2bGjSH0%2FpDXK2MrFUH3viAKDcWRwuIffj9GKhBeCJB%2FJiwcqAxyG3ZNc6uWpcioryH1HsFvIQcAqEWWWwdHl0yz5esbsU77cYDMiD8nWe%2Fa9WvMqLUxP%2Fs9sR5nxXrJ3T7uKqgAtsWVzVXQ8d7AuMuf3OfjQNXf2E%2B3uELZAT7XaaKCIlT5NQf3fyIWsLC8MSjZg8kucjLB38G57xmcChVQlW76ti5pG4OPi4UGWO8yVnc2eBAFeoaxRjuPTBfuAhFCLJDDwSnRMiWhsTCR1n4WcH9X2ehtF9SrOkfi3Fqo7%2FMEtDJER%2FKmsZTacg12VUcLKBLsQYBuQu7iEZ5j%2FnURaFyOuWgsEf4ao3wnlHSvTb9QD%2BWZgAPf7scwzB2YdBpSheMIyonMkGOqUBmvSSjSNENQDW8RTqulRIo9Cdud%2BiEeH%2BnLQPY3iTrujz%2FJAaZrAAYGgGL06%2FPJIRGzideGHtk61NwfdMc1bJ58owJalCiz77wKxxc4PbJNewRwDsHfhOn9WHfOjK5my%2Bxn6i5YD1HdxM25Dl%2Bv6M%2F35e7wf3KXKlSWpOsVayCIWhN0nMqVyqHEu3kFqQxSa003jF9WOYGN%2BOvFs7wQB%2FA84cxDtE&X-Amz-Signature=5c0eac069968b4a850fed538050a020e977e76f8e5505f16ee950a579cba0b3b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

