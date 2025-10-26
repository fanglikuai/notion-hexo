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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WQVDGUC3%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T100051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAarK9EH1yp8X%2Ba%2FDeNZCB0fEfrNNb42WPqGmBHTEVBRAiEA8jM0Zh2O7OH%2FPRs7zt23EedPpI7HM%2BrMzhnAiam0UKsqiAQIiP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAts3fPJ5yF9H9zFhCrcA3hEB14SJmXG6IdknuwP8oDudNH4wmECw3eNPu%2FsUcjL%2Bb2Ue4Ma9JxsrpSq9fllNWnjyiFP0TOaUhm85qbX6pIfNV%2F085LQWLLjlv1qICRddj6HuZVEPZOi7bX2uPcQZ%2BLohDkNhq1IwXBFzPKCf4dNriGzxdyjPkelj9fN1E50kcBTUWsGJCfAnTy89ukwI7uEtNSEMwnj4%2Bs3Oo5HrYAa5nW%2B7O3DMKvK8gYsJWdY79wUO%2BHRRCSa26YI7KCJmlJi9Yoc5oQ%2BmnJzt4ifbmQpB%2BefLcK21sat9xlYsb3dGwIbS7UO3xPJYCfergvAogvSXvgelIVLB6s99WpknLoU5ZapC64wW1uMbZtS8pWqaggJm9o0v8M5Mm%2B8zSTfgqQiiDYAKYWMMl%2Fr9dKm7VqQeIDVRVpBt0Ir9o3Q1M%2FKlFgcEgSN9G%2B5DNWOPPODdOM4LnmfhfVUNoargcTLfbmY6%2Fpy3LKhMfIIvF%2FwoqeBWHLHXsfgKwhE73X9qcT5zEVhByLU2qEbD8WatIVPL2waw1h%2BDlcTaCB0lG5mCLV28hDyqcl3ZtiTvHecTYEIFDWO1bPUthWVPjfwI7R69%2BinGBJLpM88NGl%2BmOGDM8C5y8FOcjyk4rA4o6qhMMH%2F9scGOqUBZgKl%2B8Aj7UpGQ%2BBM0njwPdioaEcp1AgNv%2Bgdppba7zoB1XPFKIxmMc0tGMICM8CqGbxGUBTQjlIM%2F2gZTzqSZWH%2FQTCtfxaM1ZfnJ58JaatsVWi5H7i9x6AJRN5g%2FcgnTq7R8f1hez6CsihWets29yFHzry95gyjz6Sz3Iwnep2xJU8UAqs63xH%2FxfFTT6M34DNOM7MXHcFRJXlywVMbpbxueG%2Bb&X-Amz-Signature=072efedec9bbdb8a5e98e75c4e0c72b8d0cd6f79de41907cead80f32d6f56b83&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

