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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662GKNT52U%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T210043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA0aCXVzLXdlc3QtMiJGMEQCIAnQ1qmNjCIPLO%2BRKbR87YtsOqf5DA8erEHMF6e7I%2BUGAiAdyR3vwQOcaqcqVJJ2YAoP6zXEJ%2FMGEyglNh9GI1%2BgKSqIBAiW%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMBvo2EYAaRGhU1XeuKtwDfphjbuYloK2pkZrySGxcEgboxEv%2FTdkf7fOO444MEURVyPurShIiMQoG6YBWOm%2B8LyoK9cGZ%2BGloQ2zflB1inNreLP3y4mJLv41LDb3tH5dT%2Bh0evUsAsqwjA9hOO1PztRlSs8tkI29at4WxX633aIKbGMa88XYTr2EtlAdPzswe5KDdGXWnEV9THlurCQv37qLMuF1BkRkfKZxjBe9nPoJseRfkF7KAiW54fjCbr5duW4s5lDvAf%2BXoCPozuqBjfsDFPJJdo%2FGJURUDkoSIPRd11qK%2Fwv3ymtvpNhjyFNuQLGVPZDHOdgbuXHPZdUKv%2BgqwBQhvbPZQchbh0GPDWeOOOkozio43L4FW%2BryAhpZiSTWqR3WlDOTJ6VF1%2BFuMXi8zxG45kNXnJzzgQu9DpL4VTBvO8YppfxUlCs%2BNVbqVj270m1Y87yCwgmQvBCRWEkbw8%2FBSMW9BbC70x1rgbq790pDHH0gpC8AYYRT02dqge5F6nlP0dQKGOj3Lc9v4oOo8EPN2vq244VlgbJI74RLSGDPKKeaYG44NFFhmr7oCRyTiKrbD7i5gAIhnGrRSaxkWelRH0LImXkCdLvPBl1BVNF279W0AfiuH0TsGz%2F8iTfWsb%2FLeEXPRKFEwmPfbxgY6pgEEiFDaRcKUv8UOfs33TzBsO7cLT2X%2FQtkkoCeFhLI2JTfkvWwINMzqyfHXTbIjqH4Uco%2FPIUFH%2FmHnH2J6kDOWSGnv3G5VwEoit%2BcJIv3hZ7CFFsD5dtOkqHYYHbwPgwe9iduo7zQpijrM4N%2FQMyz5Bzmebvd5H4eGVaHuaAEBZZZDhi2HQZZB7Yq1E7ncdttUJWn6xXyeVPfZqzjSryEae3OeRSqc&X-Amz-Signature=ed9c6eb1e9152370c6ad8b46691b21d35f2be796919f553ab441504538b9b115&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

