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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662DYZZ3UE%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T130057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC7Z174uWBP4VvT%2FoFQWRQX1ABlQtMJe5S54WIA6zdJhwIgBGFXVZ4qYPk%2FmPwhgNLhnYovPYWaKbb%2BqKcye1jCsr0q%2FwMIRhAAGgw2Mzc0MjMxODM4MDUiDAQee9jv7hKNEglj%2BircAywGes%2BhKHcRXyLspR%2FIx2W3Vf8TlgB2vEYfY0MF0e0gj50GOVDZwiyCXxuHfxNgTiC9wfZSBlJcecdPk8Alu1AR2kWA9QuAiDnjGLkv4jPf3WFum2xWbmtkJkfcb01SbhsYKTbc8W6G3fetdwP0R2AAipBBML8m1XEnlol5ULYv40d7PAsrO%2BIHydEPQRMtkKU5RZAjVtAbdD1RC4o6GEfi9SvEuBJ9%2B2RsO5U%2BpDPFD0%2BlOBZo1A3c4E%2BgQBGKQtatBSjTD%2FhGqebkzKWM4Qjwi%2B5de6L5XDw3zt2LpBf6FA%2BQKRSg2JUd8iVx%2B%2BxtecM95DV1KIw2hZwHuzJ8%2Bth2YQumdcRd9fODiYll9um6rKXK8gKS3SNaSIjiGm2J8iApddplLGTybmE%2FHURdkLnb9EiZArxL8RhSYiJK1S9f9z9ALPxpbf8%2FSmWano9hMlE3s0ZtCp2BuxzLyDajY0ZCnY27ed709kYfnau3Et6zwkac9Mi6w5c1yMZR6aQo6grKpzENbu5LzptGK5tACA4ueh6SLuIaM4G8yxwIIw3Wx5vU%2F0HQi04aincpea3czuHylN1wDDdv92WsK2M6xpTyKqXxHkU3gpTlCzoAsrh2nSHAQl3HkUHCAqxXMMyxysYGOqUBMMKkvccFTEJUHiQ%2FjeETAU%2BlH%2FnNxPmZCc0mdY1qeqpaEjdsHmg4ZyV2UZ%2B5mFGDPxEQyfAnYkLQaN2TDy8Bz9whul7I5tkEuyI4anJd3LPhspis%2Bh9rpDsD4p%2F9%2FSYaJ0SNyeygAOarj1HKJUe9cWmfZe6%2FzcvyC1BvAswdYZTnHdkLnHgVvNpu8knbOVZ3vifNkKsoonLjBnhHnMGQzGXRe8tL&X-Amz-Signature=35dffdcb225a3262f5753d3aba2d6d0c595c0ee02810fc64f211e0eba7f0141d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

