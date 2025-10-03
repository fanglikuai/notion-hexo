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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663XRQNOFR%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T140050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICj3tszmmkkQtgyJb1puE76S0DMPZlH6WFX78Qq7ZIG8AiASbIlLdERVZChYvPCcbMryajCGPJEUJwR9e3U9GTvX%2Fir%2FAwhGEAAaDDYzNzQyMzE4MzgwNSIMYlYvuD3C7mVtP%2BrhKtwDdJV%2BqC%2FD9hTxVoiXuckj5e7lyUf4d2oEoFq2%2FbnKUkwUtZsbfvd7Y8T76c4ddzQXALYQ18nMBzaZ5wDUVnczBHBLaNIecu47YjcM3K7yGRSr%2FzLj6MSxL6fvQdifdWDwKSUReEqR%2FIzqdXtFII69mRoWj4goAZ7RutndoUa1oESpLqqSDcEyI%2BPsh8H97pA9AeXwtX3Nb89OoLlecPA%2F%2FOknDRHMHZsjdoJL1%2Ft4JtP7S1yjVWpPb1jKAHZgbY3XIAs3fxcjUTKdBz8GJz6e9Dj7mIifXwmg%2BkcZGSk7mzZWoP22wrpHuSyXcdV4Zlnz0PwNtO8NjGGU20fAMVdFLycb4LJaAN1tmhpX7znv7BXJ0V9CqvC8%2FsN9n%2F7EJHHLfHMNMqYsuIzOn%2FBlwpuyhmkbAdG0j%2FmwrnHLTrzbd8webqEsjnaKRc%2FbIK3nBx9oUqcLpKnTR17%2BliZhYGJk3YIzwCG4p8zo3q8XtB8VywiRoji84%2FxNpy5rKXNAwB8B2ggnIcqJdioY24lf05x4JaATIZRYWLQVE85e07JcyMSxFOQduzb7Q14Ai5bv72e902zE0UdtyqxeXxzB3GSNdqnLPJOcxLMprcpc9Nc9zX3LLy%2Fz%2BaslZNcKKAQwmZL%2FxgY6pgHO2P%2ByrZeBhqfK6vYH1MaVNhK5Eeo0PTDUsuBdHGGeYdPqQrIA42QMk5KqY9SLw%2F6Ki5xQYXCb64r9LUyhzFCIoXOrC8UmwEU2M5UeJHAy%2BnKNSqCEYXuYpfMiyLE3QIZ6zB5DahKSkylML8JIuQKEIeoTRabifI8%2FrZSWgs3FjpcxgCFeVjkmr%2F8P4xuqATDMtwcftOBTzGErsEpisU5ZoY7SU8eT&X-Amz-Signature=069cd7f1eb319d01652ea6902dbdb13758228b49dd3100cbb790d561461307c8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

