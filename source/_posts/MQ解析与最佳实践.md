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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WWSFCUMV%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T020207Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAIaCXVzLXdlc3QtMiJHMEUCIQDDC%2B1tkAVdJ%2Fgf12Sf88A0%2FGFh6VOEP%2FTBYxV10TAbWgIgK5Ebf5AabhSYK9hoRXxLemUHIrK4WDtk7ovSwLdPoDsqiAQIy%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGfaf9iWe6W6XrdUJCrcAzyg63kF%2Fv0yEN76MnwhIteRNfRxq11G2LXi0w5H7m9c%2BdIyjVZxl7hrIpV9h6dIioWSFpKeDr1nXJYjVcqdoHgujYzIYXdi9Hl1jzTOWFswF47kur1tcIXoiPSGwiMWOLH4Ti0fDIwppkZaQiEhQ6FcHh2e6cP%2FgFDgV66PIj4HgzQzeM%2FcZhgWKhIQcPxSX3cJCxj1bvHqEugeENLzxLtlfFEaeFtQewo%2BAhkgixu%2B7blX7TgbbEmiQJBQZonbL08tQ0Yt6V3GSe%2FBSGUic2zrPM76pr9ryaPHtM7BWCl2doK3zkSEvybBR3br6pvtMUDIHuQlglosvsAbuo%2B%2BAhzKxPGhlJTr7U34pdhk7DRQxtKiCFq3tOllsRwf%2BHZC8sTd9zyGBMQ7Vy5x8J4pR1Xjd%2B74KzBnlsCLqbBFbTpPXWdk679qfXjktq4w6wA%2FB%2FiJAfsWOj8%2F3ObjEFTCuD%2FmG83Bmyeeo2ENxTiM72gQ3T1VbWVIgjym9p2SYqvk7wC5MNHrozi232AIELmhUucuZ7hud0s2lEz2AZD6mnOgRskKkNpVuntQrftxyghgUmpYMCGKlLH2fLRhE%2BoQUF9igsm89CpnJVnkwLTbTBgXFrQflVDFSuBmHFn9MIi5usgGOqUBU8BwBFxKQ9c9P57kWh075JsqxySVFUEAtx5Y5f2M5l3FFkFNdw9QQzxmISgaO02%2FOwCNt88aIAppxiXzACx2psQ5RaSRLeJ8SQTeXYN5MYNqF88sjrJ6OaMtknCBE7LbUKHjrJElyiCQ0DNiX24ywTRJTgD9SMJGFw46cQWecjOShNvbVWBriAItvAH27Kfq%2BnomYjVaPgomkE6FDPz1%2Bm9z0rlL&X-Amz-Signature=8b877e3404c78ec7f2399df1759d0da59f73d9fb6144fa72225fbdfe56ba5d28&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

