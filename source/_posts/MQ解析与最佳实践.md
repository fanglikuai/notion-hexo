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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662ZPEOUIV%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T020041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCI0BSyGKOSo5tKQVGaE6yMSRtLDc2u4M6uoscsaFJfxwIgBKbISRGICGbY%2FctFDAoE2mbikbIM5xzahR%2BOvXjGGVsq%2FwMIYxAAGgw2Mzc0MjMxODM4MDUiDEjJQlduw%2Fiz1BuyoSrcA3MSzaNUNCWh5Qsert%2BZI%2FlTVQm8Nc6NvBnGVHZnwJZj30vYiX2yciwkW%2FNBAnCk9M%2BOj2V1wTxb%2BMfp3rFWjEKnoESMAEkX5OFi9%2BbefEdQMOArt1kEAU0uuOvwcDXk81L0hZlL%2BOnsr2LNHKTUPFm8kM1NMyYbKMsNIcui2WGEGa0psTUnd0DhD%2Fk2kmMbedleo%2BT2AXTzgwMcNkm64W5FDdrpQ9RPULYTJw5ZrLiXRhD5OwwegSj9MhcK0no%2FERk0cTSwehNlyGBNAbPqfNKxMXgY2s%2FaR6ibAq01kJKBoB26XyYLD1xYfhUrEowK6yN7wfPbj7mVuO9ugYVB1ebW4C2GG8EW37bqv8Ss%2BsqK1div41SA7D2a2Cpo66N4FnH6sdmxpVOKlIRoH%2B0BQbA1H%2BdcHE2HbPrnVib3Sz%2BcJ5DKOGr%2FjvsocpccV2EB3q46hPaxCNICaNJD1Dw9qt4Tz2QEFy13QM9%2BMy8ENrwtDqkvtxCpjQ04RkS5frk%2Fzzi6B3waDF7cvLdNdZaQfDqRxjtDlzHipJFx%2B9kuhVlQkZ3tRZDCE4WX%2F9QwJA3zrHob%2F8%2Bcd2q1pBZrkN7zlBysqoTbogCl7B6But%2Fb30TUI1s%2BHHZ%2FdBU0Ebm6MJaalMkGOqUBaVdl%2BrLW7DV1gJM0JohCUyOMF5qCsW0j3t%2FJ1ENr0z0znIZwI97IlzeDCtAni8%2Fi%2BxX4icxKPgzrcPgAhP5JczBNJwXEQOUPznDOW7v6yaSNClqUg8ZRqUCbFOz%2BLsFBKCTUIWlq5VHgVnFA4XddZqnw7Q8DB02fNy%2FvNPcqP%2F8wsj6q9emelhBsTM3YwT5KyJTizsaIaOn5DOE%2F%2BCHnD9Zh8mKF&X-Amz-Signature=45060437f93359c72690c40ff1363af9cd842c77d2e2bb8a7b523269de38f9c1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

