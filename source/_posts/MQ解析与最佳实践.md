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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663ECY2HUW%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T050045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCJ2OQaU8izDq9rvbLZP7c4JNOG7eYRgLDO28VgYtz0kAIgTLE17EyIbAJzNEEIf7y%2FznANpGoL8PzKKZGdEy1sSmIqiAQIhv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJ6UoxUsqvvoV5b7XyrcA4%2FypfAkjfE2WBnvi%2FQKlzBCHVLmESGi2pbJEvsiBEGZR%2FqWMVx6nOoD7zbL6MDm7VGCztkSQVjvkWPhUnIrgDbwrS8NtJFJz2TyJk%2B%2BWW6S%2Fu1WA%2FOGPgeVbfxUUErS6YeDQUR6Ai5TNtcvPljH6JVnJ8iSqVnX%2FUQ7nRQfW2vv32uHDKIHo%2BewKBPfKwSMaT1ZnV7Wip7XGhTGJV2WsZoPR2lfiQb9BZLQw2S2b69PrS3%2FdWRggeCwjMHKCOeMCOVAa97zIKG0SJ2QFpmrhXFGPyWGqukCWLp9qAUTo1NFElD9OeMITSMZ2cl4VzYlGVhsRNEqcDqM%2B9MzXA0yt6ITeFoj%2Fx5XoNOPqCOb5%2B7qUzunEN97UjHaSq5YOJBSpOOAKNT%2FtdVwvR8Kv67Uq7h3PXVlQbyRi8ie%2F4CK2YOkbA4Dtk6RDj6u0WolZZhLlvurEqlPpxbAwxnhK9wlFTSQkfvo4%2BN94VhoowuLPs%2BMjc0Wq5J6Y%2BeWOpNDWpHGIMNCpuNr%2BlCbge%2FIGyje0WMRs70VcQ7GW9rEEETgM8Tusm7vNx7%2BEIq9%2FUCbIKqONMlQq4Glza2c2hEgvhRNJq8NynHEmy69UnNPGgQkADKdDhRilJc0otnchTXBMLC62MYGOqUBSOKGxsPu9kDDAibp7lw8JEmdo%2FnnusfTl%2BIlxdoEJWK7i%2B0KrpxqPNLhELLFYalA3gBXjqtCe6rmMuS%2BA5LRHxoYS9mhXIGqWoImQR9WIJ6REpFysiSJy82pwSzkMIgezK1wk%2BST7%2FtmUr0B3UEP6rQv%2BuIFBhVf7%2FAvFycHHYrlLH1MrtCuC9vumzjDCOAut%2BuQ7BZ4aSQJ8Ottwu%2BVpGDDsKVp&X-Amz-Signature=7ab4034329be19902023e2079f4a68e403cbac55362b203f96c789cae10ac7bc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

