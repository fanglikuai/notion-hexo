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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466257HR2OU%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T100051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJIMEYCIQDBZn%2B6Y4AkaYU3oEYVt8eEv30LDPfg8QJVfZWBKqqkVQIhAMG2PWT2rJLZgVGj9y0ZntKW2Y%2Bn%2FOlz2zUAO0KxetvHKogECPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwPZhvN1tpZhfmkDCcq3AOivPze42m1CWuYnRYkzjnpUCvWqIOU02S1R6kuG0KFnEB1UCkL6KeqGMdTv7VlvsRZXli%2FM4277teduio7nRFDL64am9FJILPHDhT9prQcLjaxw8FprH75JLQXQ9scQJzC8%2F3hDcPAlb9WhjRxkd8RVV%2ByO2UCrsiUyVsl60%2BrNDECtALVlESptvpBbnXHyXAUmo%2FzSkQkO66hq3yOYoZDtEBZ7YqbH4gYaJmOHAoxtb8RyExj2MgSjS1v6k%2FuqzQAXttfoTdaHnbp4Q8SUGZYxAWiGhtGoedhjaIRdOsJo8t2LR8LS9r5LF4g5xqbbZHhTKhR%2BQoKu0cT4Yn%2F6LoURKbXYDIC8uszV%2FdFgIF86OmtOau9B4dR9525AAXfqs6oD1LtOIDyzqCLHW542aI2GM1hXXgqNGWDkWuUiCNlyeNBl6CyiDcCvDtWDP%2BSK%2BnIYqttP617svX6W1X529MWXwFIutO3Ou320kWLRHAoBl8%2FqqZkWpXOQ0x%2F%2FatkRZ%2BjbZdOrhlc1bHz0Bo0zxcBbQ9AcB9ipzf750jARYcso74y0iyx4uGT5DTCTp14EWF6Gjm3ZxrOrWOulIEXCUZOIpzq54WiAWBQWLOElScYTUfSF7yB6kERlw37mjD8s%2FvIBjqkAZ7%2FJGPiGurSMgYJZQi04ftvAARrbc9uoL%2B%2BH6R3v0RMfTczAPso42CIjPXM9uYCGNiyBOS4Hjol2JUBrcYI%2BbgKm2XfODuWfubxBgQx31hetacU1V5L8QeCWDWVMDUHtQxUJ9fD4%2BZPUiS5ZmK4DRHVJRii8Ao8wfTWsTaPZLDwBJQx2BCVym8Fe1AEGPVr7nMpeN%2FfurWDFaf0No2BzbrdwLLA&X-Amz-Signature=919e2d45a53c79cfe5773e0de3d429d8ee97a1478f3d15554908d49ea17b3bc2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

