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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SC6JIBKL%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T050106Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCZcgsKtX8bnSiS5ldOx3kuZo6BB8ycu2yGz35qAv3POgIhAKyNtWlhbjBy7J9feJoZDilFQ8Kbe0wAeHL%2FOYxJAiTXKv8DCGUQABoMNjM3NDIzMTgzODA1IgzBaEmSKKn367GzmOYq3AM2gloLkjn646PUild3OlDkSB2GTo2TyovmtKl2uxmQNCF%2FBXsFjme%2Bu3eFpR0OAhfWEfbqnjDsSVzynRc58slCLbhJxXgMxIA9ro0tepCd4HmQgep5m95SCP9xE7O%2BPKV4bA0d%2F7tjj6eptyMLpdO3YhO4HMM38%2FGmEflZn8kx%2F%2FhSS73wsvAB8lfF6ukC4eSiA2yhQqjA3KJaMehBD9wmW33nUt4la0%2BekU01hTrS3ttDahJYdS15K5fmMzLJ3jM0K8r4tfGVgnS9uDBeuXq5yijnvTrI1sA1StE%2BVshiJjP9TFnxTRWqvQAre7lLFQ3GJyMZf6Ro5rapn%2BfFvVZZchS%2B6fkP3sB2dYCY9jIJl9gZHhbrCNgXX4CdA%2F7nLljvcQcNVXtqenUPjqq3pU8LQN0eGmLeal9rkFlPsko8K4jy09t2aj%2Fw321tVUjajU993Las2C8KGBsfNSTijXFCCKM7qfWUoXpBxApBeAIwf5R4SnbCCvX%2FtPnqfeboBKPFZv8h%2Br8IqakE1byAkHmWdEP4Bs%2BtiiLFkpdUwO%2BU6F3oL4qyMFCDg3hh9dezfRr5CUtS6m02o5obIq6XyL%2FFNrrgYpxAM4cFC2Jk9CgCCgzeKKWgIl0wXAOwRDDi2pTJBjqkAYYWS%2FlXtvAMAowMRZEgp1FIhy1UTlFPHdT9euhoi72pJ%2BfjASB6YNlBehLjvEMJAKxPjLHQTWeU%2FfjCbP6ml7608LSOK9mhSJ4gQdaEkVGRHvUxuH98nxq55OA1zJik8TYpEFzEDNAkBnVjNLKFUoBavflO4wRtEZjM4RNYS37%2FqM6txGPrIj95Np91pVoMKf4eDbsS8ZcKWXck0PkbbqzpsFkE&X-Amz-Signature=01fd9e9dafe49994f17b7b810d7c3cac04d5590daee8658b2fb52be1e1e7ffa3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

