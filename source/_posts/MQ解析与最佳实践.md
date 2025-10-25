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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663RDNPU6F%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T120050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC4Gb90RsXmSCkeQPg4pX3wCMlH8aDBbSqDc4kQevRhaQIhANRyfaVNCb5v%2FGTevxpxOSfG2NnOgn5Fx2WIZ1KCPoMKKv8DCHQQABoMNjM3NDIzMTgzODA1Igwz28IByC%2FyHpM9Zrwq3AOB2sRAsTx5A%2F2xyiKzE8mmVx7BVhXPsjjWQHWyB4eJ%2BgkTWsN7MS2mIqoX4sH5rmaMLZoiCfQ%2BBEEFoRU8GCBs1d%2B5CSBwKRtLEA3zRiy75BGvAkW%2F6NxUayOqlPJRCLNa7hWS5ZKuO6xkAuki4a12gbgdho4LQZVRPQnuCCppkWNKRIFIFwoXO%2BlbpVgW4WoxsXZNQMzmXKRUGBf8Tdz4ofQvnyo%2BaGOHKXe3g80OwgcaKLyIU%2FFJnkj5%2FABnkWk3mL39Saj7FlghywkIGATkR1vKHsnkjGeaekYwNOWHUeZS1w9Hm84dFqiGdyXyDRajvRy8GQsIuwk8JrdUnEoAkLF%2BsBIPVh1LFNeHXTtiYh7SI4oflqAZ%2F5WBtZIYdRs%2BumGHefrUpg8zrglYtYEwX0fKDGXzRmtsgUMowvDV2AsBKj%2BjR68kLr%2Bu%2BWZU8O4lTnMq0Y1LpXvngy%2FgmJg9ayD9pUzp%2BwSEsjvJcut4bcdTqThYeP%2FSvaG9CqPbCU61OOru8IuGj7eo57nzKQTzLVreHI2jwn4R6mPbr6I6tPHWzbMSNdPJzUwsuWmBgzPrPUO5OYiQA4YNu0NVytHlF6pXCHqMTJoXq7RGgmEIfo%2FuF6uWjnA%2ByE0UhjDL1%2FLHBjqkAYA2RXZUS5M6ec0AhJKppTHCiMeQReMAWuEG1voAdzDgAG0iEMvXrK5MTVWr7ftmhvxeKVrfs8QPQ%2Fj83DS6g5bgkC9hMVyQXQQJixRcYnymjnvvWQBZgKCCBaFGPW%2BHMpAAQSdxBCAPFkVJ1tRewXYfWN5mpwUIwXWC6Q7URtrHkXbRfbcVDVKrWWEPMJorRPcwgvwWfPbYVh0DpySo9DPse9dG&X-Amz-Signature=fcb4fdb7dceb529ceda12dd9aeb6f875bbb99dd1a36d2f381899adb1d6cb94e5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

