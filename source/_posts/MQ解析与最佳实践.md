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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UJYYOSDO%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T000047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGcaCXVzLXdlc3QtMiJGMEQCICemnaAOBfldpppxwRa%2BWUhpYzXNUhjaRC1Cche5QsIKAiAJGkVFJRi%2BTotLV%2BZO2vQwPo0fJqHG8ES%2BxbzepZmBcyr%2FAwgwEAAaDDYzNzQyMzE4MzgwNSIMH6uEGbKFo6%2F7uQA5KtwD59G%2F4rYNBccfi4o6Uvm8aK7nUufO%2BR9bFNs%2F2pBPM02EaSujmMoQUY3%2BA5hS0dyH3uKMxVshcxIqg9zaDKEb5HTZb%2BoNbK5UIhKpA%2FEWSXOQPYakK2a0SXkoGOgdPyPGQf4B%2FgDsKnWQU2CFj1APu5cYKNAOEkj24fCGW%2FBkdzJAjgLZ%2FD36lqYmBA0oMMxO2iunl7rSzHtSOWJwTmN6k6OvXKcRQ8YWegdKuCjFXALpQSN821XdtYgBgo%2BtXsgMbgtFoEOibFUfwT8n0EHoqG8TwPzXCheAjoC7k4l9E6DfwHHBlHuBJ8ntL8SvefIKfNltLfHtZB4FtvEc0Pb8z7m6z%2BApgvinjvhZ%2Fdof%2FZt3JkPZtEALainZYI8mbNoPrP6XR1m5JV4SfGTYg6GbX1cz769k93yxQ5JJlt30h%2FuiXRlDokXL2LEh6w%2FApbyzhvrARZrcieCV%2BRpj5D5fePF8LLMf%2FsK%2FCV62p%2BJZDsqiY66KHFGTNuSfB9D%2BvZovczACdgbQHmR3UP6XiYy8Ajy3UfkZN5xxyJj3QGDxOOGVYzarvX12UsPomc87oJUXRSsMSp3tXkxXkcGzo%2B43FBup3QQqZFgJ33ty3RVyTDCnFcpSau%2FZYW8fAyMw74GJyQY6pgHppQzmOxAden1PAwn7raoqChZZXCHHc1Un9%2FMl2G%2FDg04CNJU09x7445zpUo0%2FdcYEWAq3aqJ8aTb9e5XkpF1xHz81Gzr%2FeG4LCJ%2FxG1KnODmmQHLMa4OzMmYSFgzY93nHhEIMByE%2FupLk14IbQy89dz6kGGCAsRm3RV%2FfsgS8BnuQFbb8KoP0XsvwhBny1MbNyvju3%2BaIigOLvD%2Fhj6uxvkNnUwBu&X-Amz-Signature=e501d8a8d3a54d7a4224ec42c7cb79e1b8853d19707516e451755bb828204358&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

