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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666GLPEE7B%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T010103Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAhawnVveyr5wQpRaoWw2k7Oyo7VRLRbk2qNBQ6uINw7AiEA9QyZz%2FagNvAIlH9MgK1Fy%2FeQ%2FLCwsbenVjMUQCpC7lwq%2FwMIUhAAGgw2Mzc0MjMxODM4MDUiDKuY7%2FKUahs1puzu7CrcAxyP6C8ol0%2F%2BH6gq%2ButPyOPuILcqZkrKoHFTVrhmBl%2BQ%2BVd627Y9vzjFlCIzFSiUNpI0O3S8%2FVPavtMLjHRo%2BVodEVRjPY0zp5QUWycI4dUn5RysbSKUVIYkIcRl9xrhc6r1zo9gxIvZaQSAxUK29nRLdAaGaBemRoXYNG0k1eZsMZGgcreKd2EPT%2Bnxy%2Be0ElRP%2BskhOtXEnavhiwr6PSvnRZ1AtH57SpgUZik2HERUXJENmdEyPwJR1barTpZkusX03UekpszVZq8W7eG7Bg9Tt7zIGNihp0X2XFAndoHbxNx%2FLZrH1PzLARCTGaQX%2Bh8anr7ROkC90rT1usooiRgwXTsr9soL6GNb%2Fgy2j7AYuOCpGFPsjeol9EY4D23DrqjO5km3o7yudBGdgpHD1GWrp8vWTooLfFp2W0rPcLDKvvdGO0bn2gyoIN19hJokpl0lGm%2BgHx%2B87n4bQ04SZFoW%2FcsUXa1RCq7eEMqkW1pVraHL54i8uO6sFGLLbZiU1I2VOQhyMUfOwC2nJqpSNxu8HPRLVwjzJKAp5o%2F7EXlgSMVoDnaLG0Yb8tJtJP%2BfdfZ9LCJlEWsuUKNYMRHXK5xT7unVSYlNkzlHdrwvo0sBwWGEHFO16SBveC59MJK3tscGOqUBrRSZSgmyuuXcZ7T8g4wYHJVqwD2p99cIIugX8DtA3ysM49EcNmQ1wTvZTL3NyjAenFTvwZ1aRSuI4bvYP1Mgvemk2u2BROtsNyuTYUSsU9J7%2Fln%2BE%2F%2FTBiDIu61dtOvOiEdZ5xA0ptMOJo6kjJArISGiIhqeNTpngwgpG7soMOPTUbYSMCylsxL3YiXpYjSQkr5PbiOYVPgpm3UbtBCQ3WrXIUjS&X-Amz-Signature=2801a52ab2281348caf1808956b703a7033f4602b1a8d74daa379125d9b2f868&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

