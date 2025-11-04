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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666HABCIOW%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T140047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC4pWlUhR29xWhshMGLBJaFQ04r0rGIqWY%2Fy%2FS7865cpwIhAIXZTsBqLJYTByg5bOMN%2F6nlX%2FuBd0bOxxcbux2U92nsKv8DCHYQABoMNjM3NDIzMTgzODA1IgyrD0tCL7AKjAmOirUq3APK07DYWljlGDJRAbAHSmSZhG5LkvUrL8HPnHNITrKgOOYlHCroxy75f8CoLAOY4v9XTsFUOUtu9vkAI%2FbhdsxoK9u8jXMwObCRPBifGby%2FSirCdHOPBnI80e7az9cWxdbriJZVHOiGkMScj36pMeApzNApxScuUgr44YumoyuPVZR8agHHpgo5f%2FaVUAJJNw5usn1yHdw7qfA6g9Bm2%2BgFVlA6WCskUHzk%2FyfztnCyrOK8S4XRmziM5yIVHgJpfgW4xvfLAD2vluMjRW35tUfa%2FmfAt23996mS%2FR%2FSXt6CFG%2FUXYkBJKz5Ys6ZR663zIDcNKBiPuvg2tGXKG1bViixr%2B7qIjZ9Dfs7e0wj8Zjoiq22nWitFrKxHcSj4o%2FSiMwLT2n%2FwqkL28x5b6dRzdGal55b03VEtRpdNhy%2Fk1lw4vVpC1yEYF8VQS%2Fq8NVLYPXJZtuPdXQdF1PSTmLPEJEMxghs0dejk3kayEDRVcH3FNVc4HCInBSIC%2BcIJALAeGq8i70axLtZoPGNyhahNGGtifDlVGc3Sk%2BRw24MrFZvusogReO4ye%2Fv%2FkcDoEXQcqX%2BTwUwJt5zuZoZxmLflzzmx6BmP77T%2FgDdD1eeGgirmWV0B7tv%2BJZIPsvkvTCg86fIBjqkASILzhhmHf2gWPjbzKdS%2BDa%2BY%2FCH%2B7ZqzkCjTKiJBu3LEa%2FpwhNxUbBWVy%2B%2BD6KOjr29WA1SgfSq9fxeYX9c2aSFSfNfeDZAOaQ44aLjqpkmMLJuoRhztyi6OGGq1UGUig2xifyzMwYZyJF64N%2Fx%2F%2BUvwAZPYl1C7lJT0I%2F7%2BPi72NqUlylHbAUPfSEgcInQqEOk1oIuX7obPM2woJxOprWbPOWw&X-Amz-Signature=018094824be10c51e7214697ebae4996befc730f097c21cf78d015236a7e9f51&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

