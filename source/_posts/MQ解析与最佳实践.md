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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666F5XSYEB%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T110050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAkaCXVzLXdlc3QtMiJGMEQCIATSk00gEeuXl0GM6QQQdOzh4DKV9BAek2SsvKQzP9a%2BAiBu%2B0yjHrpWbBERLM5nTO5lMPzyJo%2Fyc4K2lPgvFCVtgyqIBAjS%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMX80nrFXwPVZco0eNKtwDtW4548qAnKqexKkoff1xW7Rjm84Vr0%2B02l6KMr6XRMsnbN7NFVLPdLdO7WWVM2uaW8ghlNXPaYp5p0%2BhkLzOcKNwRtyheRJLiqWs1VFo%2Bm94PXxehhFMkKP3JfjTZ48TyewGM0nQocCchZVVjXQIm6ZHFCx6FxpgYqAby9rLFSQNE9W8npNsB8CdSxAKiEebmIwkeEL02Nlb%2FSeMYRK1uatoqzAqAR13XG5z6EYSsf0zs2t%2FsFAo2KLpwCaE94ehZFvsB0AgoIk1LFh%2FRrrHheV%2B9FKLcyJTsgzqKH%2BGTtX5U05kwwATzIhxr5FPTkS79WpelSeJTW4VuproyKHF1uCEUid2cahw8PDvJw12H9wyZXLqng2w4eznYQ%2BHCFdqry0TGrUJCwpb8x2sEBuVZ4Ttcs3Ok9YJyw5pJm2iV5ZI2snkpPt1QbJUHSqOWduXsZdODmEgNFaDA0BF9RFBcjYSe3JH8IJysxba2fj9uxnn1hvPWhVzTWA%2FigLy%2FdjbLB6WSfhKb1EuLISVhDJhDfIyIcc6aV6Cgsj126KMD1oVnavuyBMZ%2FKXeyjtlVBFhGF2G6Dar5vV%2FZUz7Apw4SSSHW%2FgI6UG20tLXbPnjG5BaZDy4aOCNRnolGU8wrY68yAY6pgH3%2By5ff9S7ECtzke0%2B7DfCLfzTbAtNHrwTnvFTlYcik8x31NAPrazxHgawfs8Y95RzuArMDv3fF63qEX40oamQK1b4DyM50rzv3AQyXLgbBnRAUBFJkMvhzH709%2Bn1wF%2B%2BXarJ7YKSd2yVzG5B7fa9fDZo1JCpAlhjjdCDeMmr8alpe08XKYfrxOla0OjB9Lqo5dC%2FGkj%2F%2FmxgWzk9B0LIAU917n7L&X-Amz-Signature=e682cdd65d45a2e82b1bac3fe3e9b4d87a1f147ca2bd892b3cb6a583c60eabe0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

