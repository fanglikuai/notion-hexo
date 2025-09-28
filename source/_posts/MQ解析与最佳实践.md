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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TQKZFBQI%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T100037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC8aCXVzLXdlc3QtMiJHMEUCIH4iK70Vkykl83iETkgdZ4uXC%2B5FUYryGgeCrljAmVOmAiEA8BtOKrt9CgwveOefzbteHajwiKVDb%2FMEDlRtJBq%2FEFQqiAQIuP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKCr7d%2B2p6Fo%2FFPc%2FCrcA9KTR%2B8KtS0uJuEGDOXE%2FV5Fn0lIcmsaO9d6RHLbFffx0GOMM4MqnfxK%2BrHyg37KZBoy5ro3ZyK%2BARqam%2BSIb%2FrhDcdMAfYoRt9edQ3nhO2hGZQbC2JIkKe7QspGpMoJEU5wY3SMjGU0EZgLKOOtUC3lnjovTTs3cJIN0kOnrMv1uitEDsAjxED8gxpcOqUdf6U%2FILVAds3bOh5EcmBkyy5BWPA7yU4jJy5CPjXh10w9zJ8BZPj4dHio73fd%2BRupgxETcxMR%2BXVt9NyMgBWN2xeo7Rm4HwVxKMr5EhXnrPUG4g25xxrm7bkYmOWAnQNR8OYTbHYFPsED979%2FoYoinQELTFNwfgY6aS2cDbUVFPeSeEc6OGGJ3DyivtLqOYwdhEoJEk3lIlv3HAFrJeaA1wc4OjABzzuH1IMDG8sS9wOzXF16rBWtxdTAktoQtHnrAE2do8NvaUEO8KYJGdFDcjmmLkSMCRD7NNrHyhRdO2RO%2BzgClpKsnPqQYrh6x4I8CwCfebXgDQ%2B4D8xARPt9wOBTLuwg2vFGFyMb1UosY%2Bm4TwXLAGjrTF6xDBRsFKCLfBz0GhlgvgO3sat68%2FbQART%2FpQ0lXwdvTiHWoe1kXC2T4Sozzj4COC%2BkpOWPMLa648YGOqUBJQD7wVeIQ2hmKjddm5KIqyf2v4JXkZ8%2BbfB7nqdhH5Z%2B0n5w0gV3ku%2BUNOzebFuIY0veapNQPzK6sV7TFQz0IX5ThB%2B1BOawnm%2BL6TdS7eD0uCCxy2orUDZSKIzX7ewROwOBEotHAMXvtE6Wx0CwFtG%2BZdKst1QdVGXAD8d4%2FblLxxE3BDAPqhnHuAzhR1Z1AIwH%2Fsj7C%2BPTjEQnNn78cm85rPU%2B&X-Amz-Signature=ef4a8ec060f9c534ee4a1d25c7c3d42f6755254d47b9f87c469c8fb12cad05d7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

