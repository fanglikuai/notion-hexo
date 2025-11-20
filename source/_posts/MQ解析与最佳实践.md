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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666XRWQB2J%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T170046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJIMEYCIQDIismzawN0PCxGIYuMiYKj4R2jNsnlnQub5y3Vyu7i3QIhAOeNXhWJLNbf7XAW2WKO68rCP5Uf%2BXRlSQdgMK5yOOCAKogECPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyYDxobUlp0rgxjGTgq3ANcpXrRnZ6ovNthTiS%2FJ%2BSqDvwvAi3nnEPLfSr3PxN2Rp7hjXtdJYeoa%2BByhJhOJaIrPMmfbUueZ6UA%2BDU62TT%2BFTzi4M8yKc3Mh6i4p1y%2FLyrJpchluNdN0gGU67VJ3WGvfgn208sLOeHaCJGwYsdhTjr%2FXIKuCEkjrMo%2FqKVr5MgYLw3VDhy6foxNcw9Pxb5akqL9EAKdWAAnPaZyZF4eufk2E30ETdxp4LImJHfk4WRaapnZjUB5lTPkOq8rAiVZecoRPHv8ZICAvM131nMyTa0QZHWxuItuwJPum%2FKWNWdVYhYK2i2ZHT1VAieaFJ2mBBzLu%2FrD6O%2FMTN%2FZwP8USJgCnF4ArrEyoI%2F0uOZJGdDMMefEg22ypVa2p6%2Bn2SyhNSPGp0pEyisiOH6b%2BUK6%2BavEgNt%2FWlyelRhSe3F6igq0nrUX40rIo08r%2FHgOAPYlnjPVE%2F3yF2GxRZZTCsnrxmRnm%2Bb7pKzn172EA7sAduUY%2FP%2Fo%2BdkumeCfYzZq06t7vi4lz%2FhJ2%2FQbbtq15UrzYJ1ZIJmvEh20vdBaF1973%2BSxFdUSVT8ZD7OgMIS1IUO5HScWZ7g%2BYt1lUKQTKk17mli3q2hE%2Bk7ZACck%2FXP%2F94msKpOeAMx856F%2F5zD2h%2F3IBjqkAWdzP29RX1MBdGfkef8CZod3fRWdKXBw2lPLi89gJIuKevC7KtbzJQ6jSHBomjGsaoPS4C%2BKeCvRiwo0jr9lhYB3MYVZ4W2zfLrdxNTMx7FvPIzgkMECgL6Cxj1p9NpLwp%2BDQlDM%2FLiqtCKGJ5SNL2bhBaKikbqVIlQaGO9Psaq9438j%2BPP40mux3%2FQH1lPCL7dbmOjq7xuUS36reDs8IIDSSSP%2F&X-Amz-Signature=35b6c9dc6bef2346507945de5c9bc8e7171ca71eb88b7e032d3f42013f83e016&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

