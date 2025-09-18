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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SN7ZQ2B6%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T100051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED4aCXVzLXdlc3QtMiJIMEYCIQDRk%2BlIpVD7APlqCrx7S00fcaRlptvn2WMan8GpCd6PhgIhAOVtzRorLR5X%2FAunavVdBKAfZmBbNO3Geo5zgMyHD8bGKogECLf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxFFxn82MyYgDqmdZMq3AOitu0wIaW1OlrsdWqj04%2Fe9RY1ed3%2BQkEA1rFrZb61lGNbrQukOXeEy%2BryNFcjEASjCJWgHqmLwxL1N3Z4H8UGxqTAsE3YMGB5kKall44k7IK0X62xR2SItV7sNfIBnY8rEAMzaUmLkXmbyq6MhJ3lSnQxrJaGjP5slF6nHADOjOtK9vSa0X81YM5hO7s6Yg2OLxc3HJsKPVoYlJ63yJZaCcYT%2BADBcA05v3jWb%2BV%2BlPQyazCtkSiN%2Fm5%2FmcAoijYlhRL8z5vQsuwx0AryhC%2F%2BgFH6yyjlrbN7Cebdavq5RF3MlJ6JeuFwdtbD6DAgITwni5edZBWZhGlZyA2PiYwqyPrhhW5Hx%2BCXbEP%2FJfDtbPzOUK9t9nJMw1uPEUPjnUfe%2B1RoK%2FovcjLSv0uxBP%2FVlefFoAVmZZGdIekMSSVRu%2FziYIye%2FBhJnN8kAcra5PDHF43F5As5WBYaRzei%2FiIf5nAqSId0WAgsAVtxkaI7hgJAZqr5XtiGtgIjJ2quSiuUZILhTlVvzw9xYXRuW5rF2WwuypOjOMhiUNgrgjbHTyb3DaO8LdlIFZHeIVfbtbt6fW0f4p3maVZ24j3U%2BC0sYOYADM4sHSXpTLzSnI1%2BBtyuBcSbYaQDEwgDvjDkuq7GBjqkAR0PUaLRg1PVQFBFkM3McsFVbh9OldnGZoY4le4KhisjjTLG%2F9hhW525nqvkzX5FmtU64HDnlR39TD7q%2FxU%2BUfbETSF%2FBJLNV%2BqN1RF7%2B5POGNpiNogbrqQPdTOnEUJ4ezYvOvECGChwGugGcIzQu6exwG59Gbk19RwC8vZOljOaPAfe1SF4PTDW9LEvNAnwWt2K%2F8vRk05ZrscqeR23l%2Fwo%2B%2FDU&X-Amz-Signature=c6f526f9f84a128db522856a01d9ab2c4f63fed412f8a1475f13a259ddef7b88&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

