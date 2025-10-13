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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UBSVVRMC%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T110042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDGgD%2F5HPmUlfXcEn%2FbeSkg9Vvq%2F2LLjIK54HYhSBV83QIhAOnmZ1w7%2Bh84pWicvHxEeVyaUHQPIjBAKp2bdoGYpNMYKv8DCEQQABoMNjM3NDIzMTgzODA1IgwP%2FBZCmXtFF3AKmDMq3AMMhBxgZ2seESBKKZ%2BjXul6HfWdAuhBcFh5XTjS0wGUKCL1d0xQfDH0Jm2S567Pk2M9hJUJXv1wcMCrmkE5b2mNaUV3Rt6UduZNB6qi8Ha1w4j9hkR7BcEBf85XO3uhaLnl5pGWlT9AVEY9MEhTnahLoMTvrtaM5Gu%2FkmGoHxsCwMiDV3FRYBRdHVUSzEFedkr10PLeQ2DUHGXRNv15GjSNB27f60gNMAfEBFqGX7U%2BltGMSIW%2F8udB59Rg75bkVz3ZMpMa2quz6Cqu8HThvwmIcK9qgAK4HudSOmmwPhikMXdoAX8ZftyLG5KZcW8yPyCIXP9nM7H5%2BFyv1Y73mFvW2WnNgcWaJoUEnT6nkPwvt%2FXccu%2FwALdzN7ps6vY6jerdgLXfQXWfo0Qk31vwGR%2FrzyHS9GBxfeOiE34QlIfiM0ZdQ4VJUnlTiE78ZkO6wsVmT4Wv9sBrHXEnBeDErSfb6MeyvzvehaMqxpaM3Mov2rxLVUB4pEXKpC5tHYSL%2F2Z1ScK%2BbIp0Su2DGNdDgcVEl6mbCZAw555vKGIKSuHi1qjElpjScshylk58QNFG7rY2C0pXRcnSBTCN5wLngCebPYmP2HTO0%2BKFqED1yFOln3qF4IRVbQ5fWwBaDjD8pbPHBjqkAZ%2BA1FalGNhvK3467mOlWnU4CpHGhOzReI2vXnA8ve0oIkZkSGEXpPYHFZaHRVflOLhJaQ1Ys3kvIrotifkwN6GeUSpfpcFIMKxXwpOBIfvoDhkrVUiQ2WX94FbEHBi4F6rgjqKObM9ntMVZUs3HO9hpOZJPftynRRmwTUSnUBJjMq8xSugBFjifkx9q%2FKLW9xPsYspq1mcg%2FHx53%2B2XULpdGNtJ&X-Amz-Signature=7458e1c7858a18ae33d582c4875d31c882f5e587ca257c21c61d67a99741e688&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

