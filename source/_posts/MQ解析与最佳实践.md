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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664CRWALS7%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T110050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGuYHNg6%2FXJ2grbUlp6oACoyQpbx5PaUKf0To7LP1PC6AiEAra76JY%2FJxsxVz2UWYQvpN8qi3MoFyR8tccg8%2BUtryI4q%2FwMIdBAAGgw2Mzc0MjMxODM4MDUiDMY0wLCwy47GZ8sA9yrcA8wJ%2FRaynnua244pjbJ3ZZNeLkWpuCL8kkwUwJ5O06rWTRRsUcJ6fbsXgJOH0hvMusr%2FQ1QcS53HPffUqVKeQJGcrvRwq23vMAs1ylWYrE9G6c7ezzLcLNFLmthC5ghnPVefjbFUNP3HMcTOANeOltl5oZJZ7RDRFK01SHBvTVCC9OB6e0LwD3nqR%2BppEIstSn9wejv%2Fhwdgmw6qi%2BjeDpMFDafWpJpeD3jpyfLWtTP0Ikoh9m246hiMDVZm9MU9yGiZXwe%2BKHO%2B%2Fe9H33wgLI%2Fb13crM7x46l1yu29mJf1xTeeqVJkBedVexjKcAhhdLXDnqw%2FSwWUOEfrpyPxMUYhV4%2F7sIvKFC7FnCCGGiMNAJ2FnFzXQLSCMB7efx1RifL5kQhwdCU4sIcxbUHWqXxav3QHTWL0ZnurFrN73Sawk8f9RV4ZqtCZnS3iaF90w3FSTEBizbrEUcXufrv3cA%2BGp7QX3NR7iUwUjptiuyilhbTlY8Hgh9YujxUFJu85VArGG%2F1qu4FfFN5nRt0NDArhqeYassLUxKC5s4Ek7GWTyzi3Yd8EbA2xOqvQgTlE056U0qWQ09M3aB5nfxzrJVXbjXsIOVvSwsSjNiZqJXjxe1YkGMMrK4DzX5PEPMMH3vccGOqUBPGd86evhXxMHnlr3T4qKNFW0oI9AFna10MRMjG5VJyT79jlahT5hxwNH7kpA2UAKxsdHZwSKs0xq7spDJmwWAzIeHO%2BkxBrAf9QZUuG7lzDp7WEYDLFbnJqRrvxSiUIfw1G1qS6EzNqP5y0zrjKWOONa2zf%2BxNHpffp2ZS1skTSuW5mRonMbAzaMQIgUFz%2FgpsaQfGwMUCksQrLkSn9K1gN2ty%2BV&X-Amz-Signature=adced4037556bc14ba5d279bf8a31cd98294af8e670bac86173a9bb7d5d1ab47&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

