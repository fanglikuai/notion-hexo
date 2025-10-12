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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665OYLRZZ2%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T110058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIH%2F1TAQL59HzyGUYMRUlheqHEkCfM5SCYImBbhQd2KP6AiEA%2FaQtyoApy479SN0sjJcVH8kBOaxGAcD1%2BtKcwZqOPSIq%2FwMIKxAAGgw2Mzc0MjMxODM4MDUiDA4QwTdZybk7A12aVyrcA3TVeWUYa1OBDb%2FHb3y8c9DGqmOC8f0i3zisQDBZz%2FQ4dhBGd3Phmi%2ByLJXIAZBcfBQQxt0TopLs%2FH2Jvw%2Fnb6pO9HotKXjczTPFQuYFghEKjSsFVpN5xVlCrvWsTb%2Bu4dmVJMvKAWHqVyT8XPYoKWnqb2D%2Fc69ZEw3z7ffDBqpb8zP8xj5ynbUbNb25zb86UT4jHzswAOVlmKgyfQ0LTML5xEA3c3vVXVpR2E8HmNA8s92vqnxoqU%2BKB4J9e7%2BunQ3bGGjnXy3eZamp5SpDp%2BX%2FsRREpMEaEziMnLc%2BV%2BDLFxatbMIjR17gwtG%2BK1k9XgNlN%2FxpYGy3%2BFfCMl4qTBsQPzvKgdc8S1yyBw2w07sHYoLKCLny16mIDTUO2yJ8vL1sJykRJ3X6cS1kJ64C5zoHMitg4KzzZCjcJ2UlOqHvhaUD1NpHcFLWnBzqxvkCO6KbWdXa%2Bkg1dZUSEgiUWqR%2FnZ0wgZia%2FLdnAGDPZMXSdn4HTRJcuTii2jpFvG3IABhOeJwVEoVpqWxdqiNYV6Nn%2F3c1ojlagg2p%2BLBGR2tCnPdVEQ6tlsATqkhcVCO9eibTgGts2hExztnmQslkJXR5Zr6zg9FZnqm0pBInVQ5%2B%2By%2F1zOBXjW2kdrTKMI%2FsrccGOqUBkCMbpyshdUEXhsxH6OOngsJP0u97774q8eFCSBPvrNTBmVGYdGJ%2FZmihWXOBi4tVPMunDADDodBJLTTxIK6CLv2GsvXIE3VCA0dnOM%2BtalyGhlIkVUMl7%2BvFWeaPNLgVoR9P1fPSOyd2H9uR33kmZDr51lfiKrxIm%2F%2F8znsbG2lPWd4mlaX4O2eMnPvInbBjPriHSYL3MhKYCNQancKwIKcqqFiq&X-Amz-Signature=b6e993954433013c7c616757a6a4eff4ffec641df6c1a955755a2ff589f6f2c3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

