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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667EMVGAG5%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T150108Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHZlL9M7ELKHyqsLCwXXNvbxX%2BGVX%2B%2BwuIG%2Ff5LtXO%2FEAiAm7gVz0uLvwN%2F2webRuiTMVAQi2nZLeKxV8jn%2FSjcvCir%2FAwh0EAAaDDYzNzQyMzE4MzgwNSIMg9pQM8CvnZS7s6oYKtwDHr2iVCbXMbCKq30ztH7RZJy%2FOodqgk9jM5gQYpmghhbNKRWcvtVrAK71xVt77IP8whI7BBKtMwGq7gsUjpxIOAkewh7yKEeKiwQ%2BhCXTsv3NoIOrodtsE0sCdOhRtDxC4ivuPg%2BjUUTwiZzG1MzWI637wQ%2B4KFU4CQs66DUAAp95o2agPMPJP%2Bk%2FdiLxPblsk7w3inTVEaK%2BCs3ARSKbc1yQZ0SIGELY7IiP%2F%2Bb5jl1DKlpY75xenE5sAYOgA3LATWQ6BZWSZnTjycQ7%2B7u6vuodf7gfhO0WqN2WRCINJ%2Bfu6IDly0bT33D8hzrpVZSgToQsQTNoS1fLGOXSWmrNxUwI6th6pNGoZyfPNZ9Ir3Jsm3D070P6IsZr7%2B1hg8w3LtbJx%2B%2F2XugKbpaleyP%2FuBEiIXhyF2%2BCZNu9t2wbd8juS8EFfMpTAarxDMQ2KReEIIkZA3eyF7rCCkvsSVOwiCUQ6lhnyLAQdhMB4nxfoaQ9sI5TRYWuNtKZ7%2BEaY3fOJSZzYybpfTWWhtF2ZH1YDrdNm0oxV0Y3txo7IBsPRaJ3i8S6RMk9NvM0SJEDy7A%2FLIISJSzM1JmXtopXgq6%2BUpC5SLp%2F9Hc2PJBaM3h2md7WJodxe%2BLr52Bl%2Bbcwu9fyxwY6pgEkpMq05Ky2H5qzfMWsLOPbJr%2BdpVNPmFrpG%2FlIdAmMkd48W2Z8%2FyqCiyhUo7i4jc5YcwbUV3%2BYJQSA%2FC7fVCv9U4yzdNwJ5Re4lDAh9KHrmePzFaBbhZyB87Qi8Seeee1QsYt%2Fx4vE3%2B%2BEeNAVqjXKxFCLczy%2BnnDz2zR1PurwLVjbx86d9SzCM6%2BaeB1GxZBqXZWfFuD%2F2LdYti1wkMKuYAX1oJyT&X-Amz-Signature=ed73b31bea14da5de83396557369ec1ad00dca0004ae8e3b8f420cca3a7cfa6d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

