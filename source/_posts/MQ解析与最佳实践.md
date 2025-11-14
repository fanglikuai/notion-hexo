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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UMWZ7ILF%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T170052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCBj9p5U87gMsojIKXaWLpjbqNTc7aBDNbKvcABcfP6uwIgHstwa6f13g6fgZQffa07j4UFJnE1rk%2Bj9fIc%2B9%2BvgwQq%2FwMIahAAGgw2Mzc0MjMxODM4MDUiDAx%2BZBi9vmC%2Bd%2BlXeSrcA%2FH07rGW9pLmciF9mPFa6uDmaTmTxjZexrxLDAE5bURJmGIfIBX62TMsMTvHyQb7jzPwIv2tIKDKTsZnHZL20GjmGCcSSznq46dHrnuNeXYH5QQgzI8HBLTo6uzheuRX7JCWvh56IS81Igdq%2Bgb%2BXsJYCPDUlp2ZpLtDmUuJmaOJwm%2BuKYEzCfTbZDAOYoXN9vtYjhrhLdczoTbpiGrUdETIXlf94YQl9MEkKzU1Cjd520QbQj%2BxheKEqDEyLBaj0hv4LKkxN%2BdM%2FfTnZ8i9TWNIPwDtsHQPj7iy06V046qz6Es9wL%2B1lnWUfnrA1y%2BF4f6d0oHGIEF1ZOk6PHRGGJLlJMJbTwh0%2Bzjg1n16qYU5sxDc%2BhlMUEh9R7KeZsPxKbllZPxI%2BKh40Y%2FtGRDZ07PFwlncDJf4wRRy%2B6j%2BmHIGnVWaA3vVIO8d0y9%2FWx0%2FhcVP6S4OfoHU24P7Q5qkBqcIg%2FOVmzIBAFBkYO8qx5i8fEtiU0bylxmY8iEzHVS%2B0znHLUs3nBHB3fUj%2B%2B31R3xZhB4GKN3U1dLfc%2F9VsR5WJx9qjgveX8K359pkgqKWlOYDnINfdQQUBuwc3HNAsyoy0OS6KcNLkq9ZgoTPVmLsH5paV5wO6I6idC5XMJuv3cgGOqUBIp3N0rn0G0jVDLKFQep%2FXO4fIx1Hl9K9hMYDPZrf%2BP56iIQHKBjMkKF6fomd2VyFZylL2uYgxbhhhkESWgq7HzQJ4KOB02eaJOfkQLhoAWOqrmssHHBXQYoJVqq491RIpIuYwYgxd0I%2F%2F7HKISSJsx3Aba2nq7%2FbMAtsRup6EaPGRtgr9WMmDTdW51qhJ%2BTGURopdgD0xB9NGvEsr0K6RBstJwIA&X-Amz-Signature=9f0cc5dbd97e35269dd25b5490de02021ccf5343c028d84e6445b396409b5e6c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

