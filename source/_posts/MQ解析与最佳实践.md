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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662GZYHXJF%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T060047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGCrW08A9DgT5fUZjWuXfeO%2BXJZedPDcnxKv%2FHeeR42rAiA%2BNa3hq1KuP1G81soLaCJg1N1bauxEFGkZAoqu6FDBIir%2FAwhWEAAaDDYzNzQyMzE4MzgwNSIMepR%2FUGGEnh%2Bk%2FaK7KtwDIAnEMEPnmvSw1HcXvt38o7igUxKWox4d8%2FE%2Fam7eVD0HfBZVr3LsJuDyFMssHHsxFydiVbAkhZB7xTjfsSq6wytFtmdela3Ks7DCz9SjN3Bup2jX%2FmGCEBgjexZvDl9%2B0vdbrtX5DzPfbQ62CmveE09rsOsgQDNDYvgsij8pkRjxS4D0RECu6O1tU0CSBoiR6m%2BrgvPfgTXEHk%2BFwsAXNfFvsSARNb8OMUr9y6AFCNVh5cDQckFIA%2FeBVYd%2BS77Lf4TNOsZGVhGHNskeY15xUw9b4rl1CRjXSIPIE6sXbfareoPjqSXVA%2Fq0WuEJag%2F%2B%2FxFSFtYvb2fjreUeg4RPtTIW9KnliYTDBVZ4WfN7%2FTUFqoKAXjfFGOJvGeSRyY2sg9TsqhozfzbKBveoT%2F4UwC2tA8jaHcLCoaeBJOTfgyb20RHsX3cOLmkPD5WcGB0U%2FrGDfbsIpOQgR2EcWkOLHQzXgy3O0Ar6VjrjaMI1udMh%2FBLAdNUXwwoOSjzKXWbZH4KhRbAwe2X6pn39RzEHzDjNMyNCG%2FNietDMlnawzsQzP%2F%2B8GjlUu66LAGaxTD%2F6iyBQiMpHI2e8K6jX64YK70%2BEB5BnTBIH29gVNwZgBQUqvqre7WpP8KB%2Fq8EwhN6CxwY6pgE8I8gCjokdpG3jTYbvEO9X8BLrNs6nH9SVKhrvs4NQ8Ne09i%2B10mRwPxbEgfTMPy0OtC7ajZegDJXKQPhLBxz7mpbf19d%2BxkydNBIllAEYvSLHQQEj%2BFOj%2FuwOld8RpIWl0%2FWFfi0Ia3r0ZMsW3bsjnpgbj%2BbCGKTC%2Fro1EzikHDZwsxKfDvzMKXFHdn4Pf0uNNTOqAXKzgwVfENkP7s4BzuD%2BjGp3&X-Amz-Signature=cbca9374c9658edfa5de6e53277c3d5ea0db058bbee80c2391837f6e253273da&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

