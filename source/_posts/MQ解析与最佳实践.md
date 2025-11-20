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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZR3HQVTW%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T050041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECUaCXVzLXdlc3QtMiJGMEQCIG7Y4K%2FrCbd7%2FWtoPmSKTTZ3PL6xobOiM3GcdZQbsom5AiAXO3ft6%2FpSxLTqdVB%2Fqum%2BBMTBhdbrBhRZqpyzmZTV%2FSqIBAju%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM4%2FvTnIZ0%2FGfblPGzKtwDlKW0298hiyArUsRI9ABelEFevOJBZkH%2FM4076yzKF9g1Wp62cZgiB2qMVlCR%2Fib55ElYuJKcxtN5ln7CmzXLfuWavEB1bpzfJ7MDSLHvS1iFm6ioxUK7N%2FyQ%2BkIfj0J6o5tTn6u0vFlbtoCZLp%2BZ%2FernzdRItTulB%2BdR4OiUDT6P2WmnPDQjOPI2h8WLopRU%2FDVKqiwlEfueeu8%2B%2BQM%2FrqkOcbP5M42vEsP%2FmwZjp0NtVU90fkI282kTMx1ZH2MItnNlZCIvMfFCOuYiwS94H9%2B2I2pM46MPcLlXVPFE6D98gUhQZ%2FoFLkH6bXbwq7wvoqxD4EI76MUVlKMUjk4hsjZlen43tuaQ215rqAq%2Biv5G3o7FvtfXfvTUn%2FXTItPNRTlHBgMIdkiR%2BG4WysqYdJkiuI5VqPnTIMfK8p8xRLdbRgR1m2jrXnFpfpDKmN4hKoFbsCnOyV3K3giO8OGyze74qrA6Uz9dIDQdWKtNQQEW69p2Z1OPe2ztc66jaX%2BwziPCEL2qPx3aexeBfmuqF25NwAYAtgUMy4EGgYXLiDPM1doRDAaO4VFIxoY7bDL1G47uPAVQHOrTLpBHyHLFBH%2F5%2FDjntc3SeWoEzPdQheOYuzUtZZLTV5i5s%2F8wqLv6yAY6pgHU%2BJg54hT6mU7QRzlt5vyZlDHrvx1vdFKi4sglIoVR2Uca1FnbVtjDfp2cbLC34a8mDVMfZukJRfWdb6fD2IpC6tOghLXWO8mQn32Pjz4sA1f0RB2vO6lmNBnH8%2BUBDgf%2BnsxEx1OajCnb6rE8i77JNeRf2dzQoKOsoYjVzxeWOV5kkkYnHxM8edJbHl3PVwSMR810zJTFk0EdXHP%2BPu7H32HooK32&X-Amz-Signature=a76ed7c1402ca761f63109b0d7292d0503a69910dd8c9093197ef571c8cf9919&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

