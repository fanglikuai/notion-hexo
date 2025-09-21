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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663EEUS7XV%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T120051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCzJ450AQHClT6dpcApp7U6QDHdDj8IoU9xd63BtY0pnwIgUBfCLZHKB0pjYUfmzbapgsQkEBsyAjbs2Q2uN%2BxhXqkq%2FwMIExAAGgw2Mzc0MjMxODM4MDUiDLpNpRu%2BJLeMn7CiZSrcA%2FFGnQgA4%2FtTuhCTmaVW9qPSNsfRWs61dJNdnIUoV0pc0AOD0U7bupiLNo115URzZ%2FclZdgV4g92iGxjFQ7r6jEHynz5dV35iumHWxh0bTTuEeGWluWVO4SQzaUHXve0gqt%2Boajzfh2Ykl94r2nB2CkeOyPpF%2FLzGm3mo4DFzO9bo767EXV6y%2Bd5uzJDFMFsKfdRkp%2BZYVXsVM%2FwutZ%2BurmOEWLYOs3Zlvgrb4T1E2h%2BhFqE8TTYwkQHW4B5iiP%2FbosGtqoIbwC2os6imAoQL1HcJEUceEVKpxsmcaFpffHO3QkLJ3FM2U90MvKgMDII%2BbqfkH8jmsru45GjtmH8T8V%2FZzsarvhx21DV6T6NO%2FXNeqPAXDpbkb%2FykcjXs44teFoIE9aBE6IuuPGF4DtNaSDIrXQuGxECzOyDUIVm3G5gd7ZlnKhLjFwzNKUnKDQgvy7QUJhbFAHlXOnkH2DKVVfT%2FVdhTDO8lxbj7oVI%2F%2BQZ2h43jfpyHWuHak7ibVrEG09Sn3db758dNpx259xBpQ%2FOiWT%2BVTmBah359gN3GVvPDOSYW6L278ZfuPnh7Yd9YYfj06uAHeRbmKyyQ1qbj%2B%2FJYwgyjj5MaTeT6nsCwv2EQ7fmcY4O6vNEBmO0MK2cv8YGOqUBDDpNtfSad13RLNrorvhNU%2Fv8mMuRv1OOjmQNDxJMm0oSMNF5rzeeIZoj8F%2FuP%2F61qHJvUkSTqCxQZisWYIk57v5OuwlqUgna2xkGyzQtXgKX4K2ktDfpJ1x0edvDscaWW%2FPRWhA968BoYqfHlZ9uqzTy6k3hVSGQRjtd65TEkYo8M8nqvoyIwzrLZAJ6Ov850V91pdud6Ee7cPp9cXXbfzcUBZw%2B&X-Amz-Signature=e78242777d095786188d6b32714b686d6ab8a6e23e84f7fa2849a0e76814f9de&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

