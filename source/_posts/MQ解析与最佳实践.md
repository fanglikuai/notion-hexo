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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SG235KQR%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T150047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDL%2Ff0guQ40bP0GJ4TFA56TivmOCUNbARS74rFhlEDqIwIhALSXZND6Pi%2BW1WtIvJyH1YIgvyJ%2BLtbSmAKNLffLj899Kv8DCFsQABoMNjM3NDIzMTgzODA1IgybZM9iQJqc4%2F4Ujygq3AN12uneaze98miNpoNqFsRt3UFjeRY6htyGzTyeMEmqUnHNRGcf9f1LmzwiwfrtEqKM9Arp9MDifIDkKBrkYJAd%2F4DKRkXO8LXuOZbUNTm5sNHHVLpjtDbBNpty2dyNHO4zYSvpB2UUV9yLzGZDtZQviQRpB83eGqXj0Q9i747oCIZJQhCjeBW49EaM3b9yBpbb%2Fj2WQwNnPMPE5e%2FF7R78ZycL3v8x1lsCIabNf9Y4QWw9Nclj7yftMGEFnPTVSincidtbzPUxs0z0tboGEkFt9IFD6GYwBfR09oT74vlsODI2Ua%2F5M9Omz4IdEn16JGRh7F%2F5RoNC0%2BW555nOPrbrK2%2FsjV8ur9ZJOMUwPLODkZjD3imUGTEX8LbZrBvDuEZORRKjNwQiVsFUtV1S3OZdt7Z793B8%2FDivok9T47uqTBLe0ng%2B1aNUvZGtf%2FUBGpS%2F41i9dlcfOizh8gA2hlyOskVWJHcnvbOQLrSUufLdi0CC6XCd4JPEnkJ1JiEv3%2BL%2F%2BXzMpwRc2JOxEx243j3%2FvlmGB1UjDv5LNlYUNdF4QpiNbq5opWpCaP%2BqArIIzL4hvWohQVQaoCxmt3XLyvu88q3rFygdZDkOAo9QKzjRyzVpLktJowYu8Jd%2FSjC%2B4IPHBjqkAZ87jFImGZK63PvP2RGP8M73KsU52EFUsoNQFeaLtzaZoxx4L9%2Fsc1%2Bn1je94LH89xpvWSarTEw%2Fff9J06LOImEUskb8WkJGLs0hx8hKUr733%2FIpKKHOIBYNRiG%2FDxsK119qtFO0Xom7LLW4YLS%2BpR6%2Fu9ackkiF2xJeFIbS967KnNJEIyCqH2fuFH4YDjQLzLpLJSue6qg4KJ3GFjI0dUI9nhf%2F&X-Amz-Signature=b5c0248b8cab2589705fa96e72808fab897f84f3ff9c60f25d966f54cefeca16&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

