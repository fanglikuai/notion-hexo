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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V3R6K5FT%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T030041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCICK%2FeuFlPXS5r2wAaUb1Vq%2BDTGM0Jt%2B1%2FMKHlA2xe2h%2FAiEAmE%2F666PTEA7JFyrq8W94b2MNMtdGkxbt5mfw4227Io0qiAQI%2Bv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDImjtVKN0qgI%2FSuDkyrcA4XvqSumICawkTXwf%2F%2BcWCVGdYwdhbGo2DJTb%2F%2BJGjH0V%2BoXe8r4G7IccnQWOCeiNRsLL7wksW8wxXMA1LY9q6kcqyqvyxndF14Frd4iHneUk9o6n1lF9nNMV29s%2BhOHzjYS9b1ZXvl6SNuKZ73nS1bV%2FbpRM%2BVkHlOsznngHwm8KTezw5n5LgLMIimKuA8nHifDxTh1QSQc3QPfzpTEHilljuz5ac947H%2F22zwnImID%2BYnOeyj4TQI8uHQUgBOYQwlRcsQizPwkf%2Fm78aTJzTki2c0QYkOBc7ucY42JVMI%2BxIy0yQRAVaHU6ez8sld00gFXwSFKnc07Ew5Ms4dLQIFqA1ppCRfTxw4XuCa8FgEWnZHSm%2F8AtDNki3a%2F0fQ4o%2B9Bc4trzzocglstIgj6Kr555dZuToTnJMzCKf3GBvnRxDR%2F0qiK6khFtZFn7RyK85d79UydOe4U5UwB1LU9GImvCLjyqNMgsm5mtUu20%2FpzW2eRY9j9%2F93yYax5F0kvjkmp0D8lKAlC9ySWS8bEqRD8YSAGAqRM9Co0ZR4ehgywbOUX%2FUmse%2F0n%2BIoUJRJUSbg7e9sszbq7KpNsstjYnkLZRlC%2BHzFUWr7ayNgXCH%2BummAfzx2sMKP%2F5UbDMMHq8cYGOqUBrVvxRCz9DVxAa0T7f4eO5J7nYe%2BEHt1icM9snI0Sa4xsQoz9Ichx%2FkhQ55wdlAkKegkNnKuMefkU1uwZj89ECNUB%2F7dc%2BygP%2BWbNqm%2FE%2B3rlUJaTB4MMZ3lSz2IUhwR2YNoKebFkZpoGeMRVFiPJrCxBNBVSB7AR%2FT0U2iM08b%2BjC8Ng71f2psUbRTnDFeRiyBkzdKinNatCQoOm0B8K3h8%2FTxxK&X-Amz-Signature=c5a262a8e2373fb345aef4e547c6361e8c7d0ab5ce698a4e9aca5d7a361415b7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

