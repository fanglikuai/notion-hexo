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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X3H5H23D%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T130052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDNbFbekt%2FW5CVxbVEuAvpFu1fsApsVA%2Ft9umrkQ6LdpAIgN2JYD2HGSa0brMzK2VKefNFfV41s4nFIsSNnOa2yNl4q%2FwMIXhAAGgw2Mzc0MjMxODM4MDUiDE4remX5Mus9%2FZ09fSrcAyk%2BxIP50v7PtC3%2FyKjYBXE162ix%2FyDYAT7hWSuBBltYJCmHhNYdHwRyErUeLj9h2oSuZgvYaYbO6bc0LDpW0Dv07WrES%2BiJfWB0rUHthIo%2BRjZnqjYrtBDT5MBfnWb9GQ6iuWAgje1gMycUZC%2F36e7ebCxGJpnYUg%2Fm7fNQm3qPajcPVxRa%2Bj%2Bn4z9GEcTjBz2wpGB1Z0RxJiROtHmyEIcsg64R09Hvjf2RPuYZO4xy3xpA%2FlyQcMVKMyQXgoySmrellzRC88TSuD55HbXq21CT4kHHcIUeAJxF0TYpJdcQ6rnfQpaBj1uHI73wJN%2FsUQXR8LcM7y6b4H3c7PlsSzMGVnsY1vz1PESJHHY%2BkMhjmFNIIzg%2FKLE3h5OO%2B%2BXmVqZYynELB%2Bd8ZET6ZGRuxep8eMJIPtMZGdnISrUPiB9RqfogzADo%2FEJCnbxyzX7C9Kp7PoQwa2aCnyrwy0tPasKgiTlQ5RReYGRACC1Gu8hPDeR%2FfwE61ii1oW4T0v%2F3sIiNmH7x8n3MsdOoDih0P%2FTI0plgGyRx3yF0G6xj3ItxZIVt2d96DNcikLruNbFU5dc%2B%2BjVwK2OvyrAcS80CnIOyELGr%2F8hX6jcjqRpXAu2HKqMzFXjAj4UIIDVcMJnSz8YGOqUBLXxiHTJUQ2d1A4KUDFYLDVDw2qiL4LnIssSzNWLO3IMSo9K6XqS%2BhXlnv8I86IaigE8At0oHkeBGp2aOSwPPIB8uxMw%2FvlfIrZ3FAa0%2BUE3A4BSo5FRDVliCXuadfIdJFKT%2Bn43%2BIdzhsHLpW4bZiuHMXrQ%2FA846gw4X9grm1IPN2U1raM3sbCGxMWyacQRfiasm0JwAhIheRLiRbtOEyTjyXH51&X-Amz-Signature=797a561c15e82d0878ce73da881f5987947140472e76f6dfedc1e44a826c11e1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

