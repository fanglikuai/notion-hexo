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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667MCRYVDC%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T000047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBvVdKdPslJOdBTWPL6InqlcMQcuXQsD6t3f8s589ePXAiBFbbjO73FV8o12hzIAp55nOWyXnuWfciZgFOWUV%2B795yqIBAiY%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMVkCHh6Mhs7PlLW6AKtwDh7h74ATpYXHq%2Bbs2%2BhqWvJAccz%2FhDCkMtxWnPEXfSzmCaCrtuiSf%2FDRgCED86Uq2XJba6NgUFF1XKHPT5GZpBo2l7XFxLXrWnskTCCPt72gF2DQyyCyhJ9XlDhZ3tCNB3JF0fjftQarVIMFK1qHlmblfzsLrI1B5g%2F6kgjzp6qR7SOk307rEguYg16TjxTNACjfZpIp5z5UkXRB3TIQuU0PU8UQBFbbk%2Fph%2FSHcPEhHBZiRmdzbrmaAMKxee1600feqat5xTOb7LK2jAj2aVqcX1AEyjCddwcMYl9RbBebvd0JczaizIPVQVVAV%2FGU0HmIYbU5q6%2BLZ6ybxYw1J3inscYp2du3avsebwIywwbLQN3mxVkxMpRlWUEN1vC4vgVuqmoLwEhj5NeHnXfKBrLCZ0h%2BMg6851FZjTvXTTOHtd%2F4THfEoJrRSyqttqF9RFlnx5TBzIRpGIaiEVFGX2kqDQtBS%2B9kyjeIkO7qPizLIPN%2BpbAsyGN5tvT9bm1ffZkg81Ct8CyOzdit37iw0tnmMJr70E7BpgWH2EeN99Q7IaqquVBGtlPXNWeydvxBMLOBrU3MMBkFjsoyKX5ZDr5JES7FRlurMMv1AOO790nPl52%2FRmWYoS8YWkm4Iwo%2FvFxwY6pgG9ZRYI23q2ks1Edv31kU5FFyWJXoBQASAngp4sW61vN0rhlvaYfvPoUBhg7ydFV0jPi0VyBiEJUopaJtQCVg2NecvEMKLCLp81x%2BBK3rxraFmxh3fvEZFHSgASNnQxI3jO1xsiB0PFNgvAYPWA3Pf3MAbk%2FgSrLhtbo%2FL4UglWHJs9LWaQCIo6GIQJ%2FTJ%2BlMrrGD9ItwMvl6UxYpk%2FBgrM4hnq8%2BEG&X-Amz-Signature=1cc8ff6308d2f9b6d6edaff8b3f22a2fd042fc353f1ca0a41b8b3dba1dd4fc5c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

