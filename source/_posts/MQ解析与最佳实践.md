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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T3ZYXYEZ%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T180042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIA5RioBok13lnpoiWqRcVxVM0ZmZ7%2BfCQuVF2cvukdq5AiBdhmJPKwLmuaKmGQfZ67yRRjyUY597lzZD17fuM5gz%2Fir%2FAwhjEAAaDDYzNzQyMzE4MzgwNSIMBFpe4mrfDUojPwQGKtwDO0UJtfvEagtvZFJBIDtSvy5jH3qex%2FVX5snpmEkxAlwCbCrI6hQpXfX5%2BZ4owtDcFnMLQlXLp4VcHCs%2BfzmDrVZIyq0T%2BTUe3eUX4cgqEJdFLpVprV2QyhPrjoF7S8mfLp%2F%2Bzre2NPx7AoiKfLLOTf%2BpPw58l9W9MmGPB4dxzZDMqWYWxlQ6bU0q1f%2B3xxw7HRLaXT2ghllCaBuP7zcbhxv6JO5USpZgU%2Fw7KJ3Um7aDLyKWuZjsn%2FlAUbvLohIm5Yen2AZC8nsV%2FclMDrIM2PMMXgGA7ssqT5nGsKtg4LTroe4HXcfZglRIFPlVqZ2EQEcWDqKvwbnOz73D9%2FGgiYar21StazGVZzH8%2BlrVAqtS2FOXC0T2zZBw0kqJOQX8mXJXl8mFUjtdP0Tcmd1dNRs2ochgMx9CI1Z%2FRqbQrcgiDT25OLP2RFkmhKDM18av0UbpGNehtzaoumoiZmbuVenPus%2F50wJ1FWkdC%2B1YxQr5wsPLm4zfvX4BJ%2BPq7vIdHfAaj7vy44IdeliG6JI0u335iEwWqEuFCcZjV29AcGRweXZUzLIAOPcoLIIBJBE73vRNlT%2Fxzs02GmA58I%2BeU5Q3D%2F4R9HnBN1kNH9%2BN25m6lGZsGi8b6xNNPgYws5i6xwY6pgF%2Fv7Abx8Ps7a4bXTdNfA0RT%2B0Ud0zHOwqDzfxCc%2FqKnvrISn2DdwWTLZbDC0bxese4bCjfKkF7hN0bWwiaUAoe5JEYvJVXqw3GEUyXbpFtcx%2FYnSqAxJ7oYqprUMCWhulSVe95qZvASFe7LNRPCusUuWpkAUWKEQSHorXGBAbuaHpa0jD0M1YIbVjCIexh2GlOHKEuOQs%2F6SGqj7zmXH3MJji1QGQ6&X-Amz-Signature=9a14ce77961b902b02786f4399e26a1865c7b6ef361d183e73010a348da3d1c3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

