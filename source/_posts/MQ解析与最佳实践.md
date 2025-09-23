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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W3FHF3OM%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T060040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIB%2BkmZTw9CpgsymY6muL2CNG%2FfUyBDs0lPfTP5y66LFXAiBP3duOqFKMNW1JAOxixKB2ZPFir9OckU%2FA13WMRvA9DCr%2FAwg%2FEAAaDDYzNzQyMzE4MzgwNSIM83Sj4krXfQygpj6PKtwDWUS24SkKChtblgeYvSnyNMDOeEYvPvofWf8gIC0ifofaySQfBQHzaayNBYY7xxmRfth18i4rJNtOPzer%2Fe%2B8na726R8GUpN664SsDXkDzSibleu5qdFnV48Jf4kUrHL%2FnvaSDa7dyAxXguM5l5Izd2DxYcQci1ZfCKSrf2gvUsM3gGPcHpUmdvUcPsqNhO1jV0iXu1QR4evXN45n2AT5s4mpsVpAC4PQ7Ic%2B3J%2BMAXx0Mi2MES%2Bm1HXp6DemUj38bm3Vayf7tLUd90nhpwpMpAoFECzKvqcHmHw0jSeBLfcCVhFw5yDrJlivpFCy2O4lfnD72oSx%2BdsSIgJsB1RNd3gLFERgWdPedjd6FrFUz1aJGqje0c2%2FRt7ipcSiPCtSPlK2RkunmZS2JPJVOk118vN7GaxF8IN4Vf4DaAfON711ktUUHAOLyfgDqhbXi1G2RvEaXoxbjnTSe0FtLuBZHJJO6r02Ml6RHdCqmrd21gxHsIjCDOJ8AH1EIp1LBS%2BmeNwN%2BZzd99sP7aqQJrwR1WI9twZ8bijJfO0mRrHBPsL8dB9je46G%2BYub6Mwbhbu3Npd2wO7zs5AP%2Fd1MWC%2FvTBFAUJvvdB9ZCzww2A%2B8%2FhzxxKDrJXlnbOc5PsMw%2BuzIxgY6pgH8GLnj%2FhN9HAO2tskd2ueV8nn7KWeRln6RpDAxDYUQCAFX1srDc9BvLBpPtNsew6%2BHUfavBkfw9foxSYpTFFAFsyl5aZb219UwKVqnQ1vh4fW3rHM3pPIWzzLrhU%2Brjs9oACjDifoukPI4k4rm23rV7eiAHqouVvsDH3LDzXXoGH9Vmu7Ex7MAAnQIFkNcKMTdw2bSXNb7%2FRT31kk04IlpE1qPKKKN&X-Amz-Signature=b8eba961d9d25e3b73cb949c9fc68425802d03834195f7b3d1a15c7729815ff2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

