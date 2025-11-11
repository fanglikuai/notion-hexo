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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R6CDOKGW%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T210045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFwaCXVzLXdlc3QtMiJHMEUCIBdZ516OGwQlrKOQfOztAXzvWvWy6SxGeXcELGIuGNqKAiEAnQbDsGPC0SGVUqnyqnZPQuFAT1ITTIU7%2FsVGCcIFy5cq%2FwMIJRAAGgw2Mzc0MjMxODM4MDUiDC3CgI7IfnOWDH%2BrMircA%2B2wtjV9AaLxiSZZI9IvGxv4J8M%2FBoLYSFTqRDNjkSyp8vyCOFmm19csCF1jzZtNiIT72lPLo7QP%2BnH2tC%2FqxZPoxRsCSObkMTpc2UT2CsUSKPUO77MKNSgmmSZM%2BTMnVwYGTG8FFyHpQU1w1BdmRmEBV%2BNfWC36RWQnXme7g9hr7GQvbyuVY1P7VDPb7oVE76FluA%2F757biOeOJbAusFjcBdmuQGBMUrehbayyIIS6RaoBeN%2BhQYv7zY8eFgobmJ5NZZZ%2Fjd0aSg4tSQaPk6Ac8d16cJjZuSo0dJvHuByVZObUSOUmvJwkoLbfa7SfdNymr%2FYm%2BbdBrO2h5qx46p6Y2Q%2FQOL31zjcAuX2m4Nh2BJF9N9LGUQRa9ue%2B6RMVsyDTkrkVCqGfmVCmbkZfFSoZFIcXoZuoW5kZo%2BnltUY6vrlpIkuz2xDpBhyBSHd73NVVKfwtSeP9eL9Ddqu0l2MwY6qK9xjfceHRpHsPfIOOkOBfmJZsn7PqFXaZwHRMqTd%2FGBpvcXgPiSKp0E%2FHZCPtOgQw4bwkE3qgyIjYczRpOstc1iXQY6SZYbqmXvGsC1ME%2B0xiXqqm4cqewiViS4f2NjbtEerc%2Fd%2F5RwmWEzkEKCtsWRlsAEkMSbMNzMMiozsgGOqUBUPpZAucFW9ic8eoVubdVUzN3q5HXzIb6vrj6nr1cyX7HdRG%2Fa6ezP%2BjcvOalqZOiQVxaAwAFS0mOaxBSXGsG%2FjKtV5EETHLyYI%2F3rntA6Gl%2BgcraUtmIvl6bT%2BStEOrfniavW6O%2FjzwjHmFcizSIKCX3EhsWloNk1QlvxzZjRZFH21ShUzetW%2F3AJr4Wdj02JkvOC0ehHmDbcsDaKljebZ8IGCpH&X-Amz-Signature=72b12f23884d1a3a4caeb4db0533ae6d0347ed0b220c2bdec30216d6842f2d9a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

