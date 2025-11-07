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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TFFSJLBY%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T150041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGatPj0rTfbJdQZhWR7i05WApef6o4cpT9oHEJnggpoSAiAkcFwOMc1HHozeykrLHkXxfN9BFM9YKcTClsd7r7D%2FuyqIBAjA%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMoru4WSR%2BtdQGHAjsKtwDCsakW6hgncYVmP96ffT%2Bub2Okmvdw7eBcp%2B%2FYc5UnkrbTpRPm0oZBjKBxWcV2sK6VDM6AaL5d15suMBZxrOLeJIdeluyKLWuJMHM%2BsTtsoJensV3sSDRgD5f4uciyfol%2BxANKHTAGh6m%2B1po1PnSxB%2Fgc10kV5pZfgAPFGjqSButJAOQut6jnWTBGoqeG%2FbNKj6iXIZ2M%2F%2Bkcx9YHmoEx9y1CQxSMSutRYeHxYUJmhSmOVbLZnPLZxNgU4DXU0Ws%2FK%2FocXS0ZF%2FsKD9XG1IBDpW8iJR%2BEE6exjJsRft51fwaY7AovrXafbEQ9sYnpe4DeLm4KvHywIGhMRZ0F7ipD%2Bnv%2Bq%2FWa4pLe6duPQi59GM9W4yMESHzVMwg8v725PTjkq79a%2BnLS0C1DnOpZW0l%2Bq3t1UTHDQ33crdnHX6TnoxRfeR%2F3%2F0YpGw9muIs6lQYssRgUtm99v2uZP7n6VBRNg3yUOEUarufLm77CCZAuZM9i85ErXKdoAxu%2FMRx1WqQcl2GYWXRqzl381G8yvnqyfZN9%2F43gAF3uECZI4JA5qKFT0V%2BFs2aue2AfsYiovfAKsHko6v6zWpQTWddcKruzaYA1VH1RrcmrjetLmMEhlShWz00IhxTqgVJIfUwsIq4yAY6pgHrhpvJQ%2FDncOTzsKcmV18Wq73gOzwKc0ju6YA%2BhccyYZ5EaDbshIUnIblqe%2Bx%2B%2Frz5cb63LcWBipNHi4EaN4HWlnRhOtNzQsxCQmR97qxx5edAsNu%2FyAJP9v8UxewU5jL%2BGa1MhRb%2F6RS8e5GVBN54RfXVt%2FwQsh3pHiZN1PgOn4ACNcKzZVC9fKMIPKigL%2BoUHhFnHLU%2BHDlszjn22UPKtZ8HjlQk&X-Amz-Signature=e529aa06b5eb00ff014b853a4d64ffdb04813bb286b7b854b8e2862e8157f484&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

