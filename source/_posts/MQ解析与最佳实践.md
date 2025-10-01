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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SQDSIY7H%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T000041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG8aCXVzLXdlc3QtMiJIMEYCIQDa66qMcwSlMWxzoFb%2FZ0elNhlTsRtzZCDZMtfr8k0TgwIhAPwjwCNQsTqqI3g3pkcerbdyPUd03m0nfsokC6orIRFpKogECPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzDjIweP4yynyv22g4q3AODz%2Bel0We08otVcX9LTy%2FOwcoPY5E30TXjaT2uJKxxHC%2FdeeiAUoR%2FbwDAVqSJFouvdvlxpG7Q9XcVT7jf2pPoouDscbvyg99kDw8%2FilAssz%2F58QuCXmqalIFY8UMuGq%2BTSKMBtOfmSTYC4ZtvKZBIo181EfnvTqPxGseg6NmCeac9civdgUbHb0IDrEZRMskfqul0h7JODp5ftM4Wr8OaeqJGuvdmqG2axWBvDx0zmJm1MX0apuauvnIS8AiA14cemeEl2Wm%2FcfYSLb66A40Ze%2F1Yl3u78O6pVHOEUQxiktIhfBz7meaW4gA01p8qjgG6jgPmRg4GLPkFAg8cwsR6ej0KTgQvf5U5h0Ig%2FBluDgdmdRvStd6LlOBpz4M6%2BXSL2lPI2YdW7ZVEGpTKTRdbRqsZne1VJPCFqJ9fpK1uCHjHwBMBK8Ig%2B%2B4eieL3aaTMdQBgFl1iQlpNGxw9ofNT8p30xP8bf0EWjLL4XWArCoHWM0tNxNsWtu2L0SvL%2BkZIp2Hb9EgyJ%2B%2FMNxb7G%2F%2BA85KdGCG7ilmf2mHs2z0nZMPVyi6S%2FdmwHyGQhY8mykcFL7v3EV6EWWDWhGLdJl%2F4aFcTRRjxso7XF0I0IUwwhkeFyZi8JL3ZlqvNgzCiw%2FHGBjqkARBKrdZHNjg983SnaELnHpASpBS%2BL2RnHVUJeTX5ovSHwGr8Bkq%2FjLi3XJzg8pTGpdHRDTUFKM6SbtFCjWacmBvKNQC%2BqO1G9Mfi3iRiKa7c6MpSmTxBNfjbXfjqOKyXPGRJdINlwr8RuNEKqx9FibJFOHsdUEJSJYo6%2B5BSb1vy8rQi9X4bRb%2Fg17axm8GrjvOt0FNT%2BQ%2BLZoX%2FwukfAMN8tN%2Fi&X-Amz-Signature=1a8d7d9dcdaf63da8819013c6421fcb1ddd99778282e9194f78183f8f24046ea&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

