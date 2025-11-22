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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQZOVCAX%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T050053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJGMEQCIDxkSOaoGPWetFSBnk3GvPYR57P9tGf9uLC%2BAx%2BSEobjAiBLY9siAODTvtcYUZYrNduPfMnWfCk%2BcF0Dg51x4ClOXyr%2FAwgeEAAaDDYzNzQyMzE4MzgwNSIMQESNSsrZR7i3eje%2FKtwDSoyVsKnPsTc51IzwtM7yzq3jCz1TP%2FO16A0RC71OzDPisfQ1rIEk7RCwOlBXsmyL2NBDAfpzt74JjKwbOhxWFqjzCRAWZq7jd0U7uS8kd7%2B4WjwaNp8qHS%2BCj0LSLUaZHAEFxYZg2GhaV6GiLQMxfMkVcaZVFzclLqK9qiMVETyjZkQkEeWcCyWHykRguNFRnFt41aYWjrDNYgufpAlk1cV1uKxr3WcIy2A90WDv2mVdM6ALVCfEDEiaG4X%2BA%2BNmTD2ZvubSy12uSSHHWiLR7NEs0G1MJkakRXWGBbSlLxmKABz7%2F9YKOfYhWIjrLAKtq%2BVeepsDTRrJVnKtTyYRm1wmlYsvMqe55D%2F1xh2OzSxR7YHysA8tk0o27KjK5OUc%2FUiKvWfEoLQKx%2FTA3wZzVDRczu7i3r4HrkZ3pJy1vJxkJ9eq7HJai3ukAY1eJ6DbbcI8IWp9SoGOYyaA2VPk4RrA71kR0qDfp7McfPuSEmD12uR6gsetmmigkv5p%2BGblPjkJlto%2Fl3mEXACgawOaf44%2FPUctQP2XC2iRUclPoC8iKznf2HnTU98j0QAYOjzc0kqTcknGzRfwZ%2BCQ1K9j3WAq%2BM%2FbkSZmrYt3DwRBiTmG2dmN2HwTmTI7Yg4w34OFyQY6pgHS%2BCmEDO6sIp0sixz2fG4ifsjyWwQ9bYuvDeMBVo8diKbaBm952UvGuqtmHwx5beW%2F6IswlgmNamjqbYTuF7dDN3aI6iZp%2FdCzhfohxRX8zQaZREOCg%2Bfl3YcwEXL4zLjkChE5I3tzPbmgVo7IjCYjYX%2FQfvXFRnwz69M%2F9e9Q7ss6tffyGPbNM5HUvo8f1Be9et1WofXHWt5jwNQA1Br4W%2B89g2%2Fd&X-Amz-Signature=9624dbc60a0791405e764f4d9bc1db41241425404b3c70be292340e571d18e8a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

