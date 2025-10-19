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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667T4KLVXD%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T110043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaCXVzLXdlc3QtMiJIMEYCIQCFYsARMgoPgFt9D9IoX7EWzDzlUKTzG1afi0G%2FMmag%2FwIhAOWDhub73RS0iXc7bJGdA6sOMh%2FcIeioML%2F6zun033ouKogECND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzbGhpGP84Bc6KIYPIq3APwrT%2F1rc5bnDSKdrEVj9cvugrqyoiI3x9pVczjQ3ILhbOJXFgpKkX4ozxNauBER4JxW3ar3NKlLuP28CYIot3V%2FOp3rD0uHCoCWQjz6hVUrUeqdIdVQDE55jwnQcZavgdoVwrBcZMRXxdh0JMnnbxH2oaSjKqvt88oW0TEdAg%2Bw1NJt7LMR4XQQhW51T8Wxx9fEji6LE094GqS73gRo1W6TUuXFdf%2FghLM1m7jJJMTBIotl57TclWSEVOgeA0L632gh4oC%2BaenttURwoW83cuEp21MVcMX1v6AX%2Bp%2FBCE9lfjvXo0dzNW8SAdA78kXFNwJRqNW%2FoBNI4PupzRiY4M9AcyM1n11cdW8L5xdF7g9KL%2BKXJnk6lp%2F1jNFLupWWMk5eVJN%2FiNb54ezijt%2BFHmZ9AlifvUarr7qdjP%2B7H3iAiwQ4eg1TBmHFRLdh4O%2BSU%2B13LuRdHx2WPBXs%2Fi8Us4Z97YhzxHzz%2BCHhiYMpZA5%2F57FlYeCwF%2Bcuq80atc2onpEurghBnNZtBf%2FgY5XCy5gz3QbcSMjmSZu3%2FNIfqWS1hov0guZmfrn%2BOdmE5XPjFwn7XK5FBrcyPXfuI4Avm9z6%2F3Kez39%2F8Z9N7KyEZHAgrYqxC1N4qp0bHUw3DC6jNLHBjqkAUN5vyLVqqK0BfaXKNwroIsBJJLxeqqYibGshdxEcXj3kj1GSyVxdlZ8BD8tBrEIrRO7LqvsFxwHq1QBFUzjX2EFcLObeLrO9sTnbTTWKIyCEn3i4ImWBfLw9utgWLAb%2BFtQNcOoKsHfjMC3T1%2FBVB5bW%2F86SwhRd7Rrm1ihPO385lgU9FeCzjt7fcXf2TJ%2F2sMo6j0P5fkFnokyG9FAHHbAH5JV&X-Amz-Signature=df58927d06c00ee92ecc3a05fd1f89c97179289f62c86da068814e45247b0fc9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

