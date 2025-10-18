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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46673RAWY5J%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T130101Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBQaCXVzLXdlc3QtMiJHMEUCIQDWLThJG2R89%2FnxDSGKZQ2EAcHKKp2gvR6AjSbZudJSXwIgPWUrcMrUZR%2B9nFvEbvKg8dCES%2FZ600ZvI%2BXTft2B3loqiAQIvf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNG5Cg2f435P1IoX9CrcAzN4zsLH1A39OM9WK6ou%2BxLOn%2BZ16fHtSBLSQk4MpxWkhveXQdXY7PSNfWxNOFmBjVXFFXOmEEXZ%2Bw83sydkReTYRKLZvS8ThUqXhTfso6b%2FFgNeNxXexaV5nWW7fyj2y21LcjJzVb75sArURSEpu7AirbTPuggu3rCdP%2Fl6IfEOGOVJR4XVIcPF%2By1DkywrSpAqibCEVfUnzKN3PBTwAiD4xXTAwTFclHZQXxa3ouy9%2F9rM2jF9cFSgrccq09xKo2t7OzKHcxktTxU4pqDnnHkLyNFgLldjJyZPoemZXpTvuqtZK6ac%2F1al4VibVgLylLWD8bmAy%2Bc3Ouiby0qzGWPzsROJG0TCfM9V1rPRal56S4luIHiWgZkOseVrYBzHgD5chGHvnz1g6twYfrWVYYGeBwDotLRPTr2TX7aQKb0S2PtLf1hVm65sTdjOzuVhVJWKXcLCbMLQ9o8A3pIsrfBCaG5XA7tZ6W7oH59o8mzkTCyPsdO3ydfkwfoseuGFhlZ1nzphFWkO2mdE3M66m%2FNiF6VuqwwNpQSvbWCCRsJMqML7V0%2B%2Bl6SOeD2n7J4Je0wjDtFS%2BFIHs4GHjhzxJqHMVVPgC2q%2BDHwrWm%2BHO4Cxvgq0enPjNmtKt37gMP%2BCzscGOqUBJxoy%2BzHjPT%2B7pxOJUHS4KITKaZ6pEKy4eCWfT4x8wwYduFmr%2BvXY6FPTvdUnXE2DhtoACaL%2BNPw0Shv%2BXm9VJffp99G8%2FFCclsOcrAZNBXJxC7K4JKw3uPNxaaxvsf842Df7JtExidR%2BkiGF4glPk7lP9p3FsM%2FjM%2Bx%2BwC%2BFA%2B8OFlfyw7KnXw1eEn%2BJ9mTw7QmsfSClDNKFIMlsS%2FFnKWvvYdsm&X-Amz-Signature=7925072e5cd87fa7c313d19606578da67449b10bf7f1b80ef11b944a8fc5d0d3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

