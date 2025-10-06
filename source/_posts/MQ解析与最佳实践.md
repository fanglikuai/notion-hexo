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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667HFJLQGL%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T100038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDXwRfNHMXEwvnG9%2BuaPheaqL7%2FIOwUcY15V8A3T0KWxAIhAJxTslicFmizZF1IK4ZtAVghhT2Ao3e9SG%2B9UNgyEdmdKogECIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igzy77JCxrLDSgBxcmgq3ANM8T%2FLwskN7%2BHQVGpSF9fQXlGDRB1bxJqjt3FXrJq7Bfjx6Ts9juE1Z9i0xwCU%2BaphPGf%2BIqXOkkJpod%2Bqh%2BiwwX6SxTpvo5hl0De5F8dTr%2BD%2FmG%2F2N4vtHWx%2FxQObydaeDGEOE2PdAfalJ%2B7%2F4enVr0wciq%2FMpYcQ8iRWVfCHmgKCrp32atfkRrKyPnn0eefdSZqXXi8mrqnhqlyW%2BOZnUNYi5f0eB7sQNcAWEHBT8UuXtnLovGI64sy2U8JEwV5%2FgefcAwurs%2FWQl3N6zZbTQrJHQCd0sTIYV8RhtmjRRXT9L0EhNuXwfZE4v9%2F%2FWQVCxjyobvUMPFvV9uOvExXnxD3xZE9c1d6MrvkKMwFPiLv%2F0CMmpC0wo7pMMtVoqGjcuFmriYWBPpEoGO%2F0eswhfXAGPhfbDqYhoAeKUmFtAa7dAQOPnfR0Udw2xNuU%2Fz2AmfwUn6uoy681H6nCc41XCOrr%2Bj1NwMn8HoZ3hkyc3fU0l4sK50Coj4dxpbe8MHO391xC2gP5FJVcpKORkdoG2a2GBudANfeKJW%2FSkVSzzEGFJmMssGn1mcdIQfgOLVyNJ4gpsQgotRLLzCFXAxB%2FNl0jhQHnBFsk7PRyFemr7qJkQmOUbtOzTk9zlTDWj47HBjqkASwhdIh4rw7IkQZ%2FFHPiZ1Z3MwZfuN7Zbnse8Icurx9vcMuZnQ5JOe4CJsfyDoe2mIVjiYjYfDLYmWwPcUtTokeNdrLxo%2Bq5c8yygBMrH3KooUxIkuESnIABALui7bJ3s44f1dCKyy8w2QP8ynbTTJBf1IJRswcnwnJrEVgZmDbz6A%2F4X2LagapOH7s0elrAXkuUfMfrUng27aWLAp8mGdSESXeK&X-Amz-Signature=e8c780de338419e0feaf609ea2a75ed1ed07d7c2450fe63d222bf18707f01ce7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

