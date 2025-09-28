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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665PZRXHQQ%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T090046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC8aCXVzLXdlc3QtMiJHMEUCIFinWHJTrLEXglPFDKZL2jG1yu5%2F2IfTnpInGQtl9jQyAiEAkCfOqq%2FLOWFxGqazD4bWa916mBqUYCWDKD%2F0BxVFzrYqiAQIuP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDO617%2BH6245SIItvtyrcA78nZOamn9II3ZKoM8gJkbBl3Sla3qwqBX2pzHCWaYeX%2BA9lbq%2FZZwThcgSxq%2Fsqnr%2FCWqnD3sMXbU7Rglvv3r5qQ6ilXbab7chqK%2F3suc%2FYdXQ%2BIOPX%2FOSIzJKxWl0tnCJspE6cosYkvQV%2BP8y75l1KwKMMEwzCjR8CIqkfVLQy0IcQvYfYfCN3T4JCFRAIjznWXJ8PfFehbE1kqfMQJrGiztuf66UC7OIlKAn5UCryor6oEmWsH1q8mDdcx1GI1Qsvqgt6AWd%2BvmOZe8s3O8rhOHbcDfFfPceTVOKEybxjsTKyBSff8Nsm9KSnrCUvjm2nnum2cya0%2Fas6veiNxpSO%2BdlsosFqzqkFnKQmD3ovVotW%2FwlP3LOrW7DoX32SgTMxWzbPqNhNXSPAOZQYTZyEUrNCYWdumZusJv9KrfBwWGn6AnED3Gzq2bZlkA4YbkSfFqWaLdX%2BEY9qnNZLnGwL7qyV5STlTnEx3tJWJ09jgFj8A3vqYceR0hp6YqzWrERfJpX%2FCysaohvQtQtdeNF5V05bocJY%2BCtlhZl9FXN2qE8Cb21VyVhKeznjr0050meOdwuUE3QUEUMJ1p7pRCylfe7rqnceXgcYbfnYidXsagAPM0U4n9KFkF6rMMS848YGOqUBQApt8Uqvax730iI%2FyMfa1OdALYubgV%2FcrSjvZQUTAfpsWUbnqjWfKtWckxzPanQXkIYngHbdZhG82KX0VWVrYnTj1F4EfA85WcPW0tnRr4JQYpnhoiDQ2x6gi9JgjyYGu6QJdLXX9BkD2kpboZrTsAzIzijwZ%2FkWjaaCC61G5X9RdnvHebYXAae6lbi%2FysI0feIq5bdBtlhzEOKKslsHP6X%2BnIyz&X-Amz-Signature=bf3445240dda20e145f1ae8795b5b626ebba21719bb3d924fd22c8bc72cafaa7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

