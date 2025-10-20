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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666FKD4M4V%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T140040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEUaCXVzLXdlc3QtMiJGMEQCIHlowWt8vQTeAvLkS9VNGi6h9VEt5fdNb%2B%2B16D3pnYNaAiA3f%2BJ9tNt5PM4OBkFJCgB6sZAPojClH1uEy16d97haRSqIBAju%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMkqcvuC3KQZ%2BIeK5%2BKtwDMjKIL3v81wftvfI6Il4QrtKUDWkrvcrm64nTtCghvo4RDbSsFcTHo53J%2FsUu4jfU0%2BxSVfex1N8ddkHMuiH7CFiVdsZXk4k94LUB9%2BLpGTVKq30dRJhVaKzm7OvTlxJKiWN6Ii6foft%2F7a5GQTOgkH7gh90ni43fWqqQF1dcfnmKLdPR%2BF4vuuBKxN0CFtRsr5xP4r17XMjDq0mSNwdOv3XGnXycv%2Fe4qEtCg15yJnfotrvyXxVsxoC5nyjQidP344sz1nndXEp1qkS1SVwwMFy3ZD7OCenVNlVGrJf9S2K6a6tj4oMr0U9E4UlpMfV8y7rkAq5r9cxF67zw3skTysQUQys8EloyAOn24BUuIvmnbkSr6UUH3DTMowK1pVsj9VBB386i1NGfjk3mRDarbQ3B%2Br3jiswXy24rc6NZYctRR84HrT6bv3W0JtLkq8tmeUFECOJTJacqqoVUxcSvFhhXOqRU59SDKOef84ojBcH8RcpiT3FsdArHi%2BtzRVyfrzkGJjg%2B6bSXqMDnU9l77EO3Lh9hgloYYVnatiEgKs99NMB7zE0VaPAiUHCFNVdvLfMrtEcVqyUNntcj2y571R5ynXHY77MjGl44B0puXCEoBoTM%2BuN0vcoDfrQwg9nYxwY6pgHncS16y1gC6USYUzBUWOFBaur5%2FwS4ypP53qmbbzWEK7BG%2BAwphBPzDO0iZJLEsoDDBzz0tRybdqdR8M%2BSNnFCTkijwCu81NDNhUMK1bKHDJQSIFdXMo7EL50ZRCJ4RMIf7I2H9urLHcs3WxwLt0N9Doz7BIDTlUU3ebeYlDOpILpwXDzTobU92ETKJSVIa6ZSNTH%2FAiKWq931TlYq91X3ezeMkZkw&X-Amz-Signature=bdabdaf2f5c9df5528783fa1bd4fc89d088da103d9a59dd14a44b410aada2a65&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

