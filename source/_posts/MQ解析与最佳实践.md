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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RNBP7Z6I%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T080048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED8aCXVzLXdlc3QtMiJHMEUCIE93b6eScioPXSIOTV95Vm1uxTO0YiGUjpDFvKLTFTmIAiEAn1VpM7mAAUt7770KndaeKFsDlAIsLlVHAWauoU3MUtAq%2FwMICBAAGgw2Mzc0MjMxODM4MDUiDLNQdfydCv8KeNj32CrcA7FFGo4Ra7kNKCgxiZD6G%2BiNZ6pfqNS2ZyVpyDoMmUNz4jB%2BRCAG%2F7SQ48UqZWpG%2F2QEzPTPCRCnn%2F51DUr1wyc%2Fes%2FptMD0W0M8CMv7KrsBS34D41H7Cvw%2FVWbvWqHHg%2B7SeRKz4bmVm6zop9d1sLWXcajsJyqpe40OlaxE3hkydCw2jraRNpVYNJaPvZ0UBuCyNdJ6y5zxc2KzCh3RgAmpgwmvkQ%2BHwDCap7FneDDa1H6rj7UFc3zBM7HhKfmXpMbe8gDlF3%2F%2BsNx%2FoauHGK43ZdYIECdvFMyF6rR5wfLQO2zE0jd5dX8tiRH0gK6R46J%2FNFd0zoAdGuJbKgYbYOOaJpG%2FVdcK37Sf%2FBNrKcGEWygAkctojBNi%2FGwZ6GeiuN3rAUAnjtzVfdPtYwyEUh7Gu8QyCvRRscFevtL2YqapvsKEmajmM1Or9auCZNSK45d%2FdyT%2Baxbs04wkVLYDxE0F6Emu33ow0aj7mE4N8OIVhnXai52lzcq%2BR%2FT6gvQBJsDc6EBGtlSOxBO0Kt5W14xftBh90%2FvljuzI%2B678yE1YDOEu1mf1bDVM5a9AJx2MeVgpy4Fj4ZKw%2FRaRuwpa8%2BdsN89KYdn5oecAB7JFJWrFXtrZWyvFXoBBIzxYMK6fgMkGOqUBuLCN%2FZkZrqZWDj8r2kjw4%2F%2BZKGhDhR%2FH4QVp1CFZB6dDe60r%2FqSfH%2B%2FcbHIfpt%2BjqVWfw4NI24M%2Ft0QKjw63O0N10m6gyRIXnNZUVRGgR7E7nmulzdjNwebnVfvGju5DEAFhICo%2Fe6MQ32M4XIuOqrcsZ61EhBIvU3QAOn60WoTHEbueOly9uFRs9ErLhoIK7SWWWsIQbviY7pOMu4FZbK2WF0j6&X-Amz-Signature=1f1a9a0ac55f2a9b49485607c0af65f38c1baf53065bd7b8fe4a9a3b2fa7c85c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

