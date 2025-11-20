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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662PFZRBJJ%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T160041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDAaCXVzLXdlc3QtMiJHMEUCIAggfKg0o0sVLe173uLMsIXBQlp7QYeO%2BfJzuJB2MkkkAiEA%2FJs1dvlH1kX4I%2BPZxF13FiiTb58%2FfVhYgNtvd2ogy%2FkqiAQI%2Bf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBoKmwRiLKX5sGfO4yrcA5k17z1sshgbz%2BuwjBbU9HfOn3jM7i6k27S28ZSBNZ4iSl2Q6qDNdZo8HG4wuUnI7KUbF9rSw4zzmD4AWVt6o9U99Ns9bHawBk4TJ4lKpphF%2FI6l%2FMhJrOvSnamr408evL4ttn5svQsjsb%2FNGUS7%2BC2GT3CnnupA%2Bwc0kFoTrLln%2BoegPqhHLfUFoTFKB7WIzSc2H%2FT2C7EhxD%2FL7V5Tso9qhszRkAkJkiUeuohwAmRAo9avaWhFJ66qMfMqSWWuM2Ec%2FrsGTahnGcvypVYbjaqkbXZ%2FWmhxbRMast3gR2X%2B6qxfXVbxnhVFgdciAyHRtczbZ9EiaPdcNbVx%2BXXk%2BtSzwhTqAXGcoFShldz9XcQ7m5ZxTh20XbWHdZICcMUp3q9x9vWEewfezLDfCINz8m%2BC0dtI7kTBMqyX3YYc6KOhwFZUVNPshmwL3Wy7s0xBlzP2duHgcYlzbx88vPvoNVoRyAPkaEFoDdNx78%2FoHUr0SsP4CT%2FA3Uv1Kws1%2BkbwLKAJBdVToZD0Yz2h22sUcvHMKfzlp7mAKixpxnwRWhK3oTowsHmfcSpGOx%2F5dEzskgleufkb38n%2B%2B1ZoDb28NfQpwukw1HRjxNahqxKth6khCLS4mnvWWp%2Bj6ppTMLTq%2FMgGOqUBBWXaQFuehHQ4pzqIcut18YYdpcCyv3WdarRzZXOrZ%2Faq16kjffcHysThlcgNaG6V%2FcyMyKYlLfD8XDsZPKkg%2BW9ti1X7Psx9O6JxM1LdtkzA2nnH6P2m5LzqvyXepw4T0sTdUaKv87owPjYlt5Ev0y9jWpyMw51Wij6rLh29XtQew1kyX%2Fvx%2F776hFRm%2F3s1fj8qaamgK7ZSSGFRNXOwUPhrRZlO&X-Amz-Signature=e8aa6ed59d5282fd3b057e10fc4279eea78dcf58988295e1de1095e21d4b1ec6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

