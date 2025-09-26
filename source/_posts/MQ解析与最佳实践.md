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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663FKH57VL%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T130101Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAQaCXVzLXdlc3QtMiJHMEUCIAMjC4bpjl%2ByrgR0fsVmWgy%2BxBvM4QrUUqdthRsHjR9VAiEA0ZPSdZZ%2BeZPRwHYPLtSOatebEhv8et%2BeHZx4CTB%2FWnoqiAQIjf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNYCFVWxbx%2Fkwa9RUCrcA%2B%2ByzQUsWOld67fkhxRt5H4s8Vy%2F0b8FrNz%2FmGa2qEfeLJ9t2yzUMBhssbDbvXqkyFlKciuwTBPt8u4QSh%2FHnvZjqbfCsrUZ3vk5QCHF6dl1Vu6k%2B9xJW%2B%2BA9GGTumXO15DLC3Rgu0Va3kp4OacJVAp558vR9MNrE2q0pcTHoNmUwajfJkDYm04CNB2rz%2FZCNZH%2BW2lnP1HrP09tisKczyu1ewhEutGA7W0eeLW1dBwDrnOP4mKFvdpRMyooY%2FZCSBRjO7YDEb3ht8XQHgPuvcfXlKVFGitHJAoQymOhWkw%2B%2BNgdxy1AKsLIUeNELEHN8ee8KZp8GrfHoFtVhCM2hlcNfjNMpn6GjaPgeeA7WY6sTVaqOOxRscTq6RVnav6V%2BbubSCIL0bp7%2FR42EUBCnwNgqzwL0XdV0WfZivT0AciHHOPRCVD0IlmsolHv14ZFDAHB%2BL1VZPvloIsSzE%2BUtpGOfFpg%2Fa0HMxrN11xMlevefGdiNrKRbm3f2sblPzMbO%2B0TqDU83wm4VYQK%2BYEmGemTZPSFT21%2BWXVrbR%2BPHTj5J2nwK78qB3up%2BVE0dL8QJgoZigxAxHzCyMHqmea9%2FcJc82ncSUEbnaq3WCFpBfvP7N8%2BJb9hLjpYR%2ByXMIr52cYGOqUBPPClQy0Bti7iyMqVmvNRXqlSph%2BSMxXynZy1HjOqCVb1RDtvwYF1NruPB4josjpv5zfJFZ3TrPwlgMTEulFz%2F8MaA2e%2Feu8de%2B98%2FHNoiegNg9t8NH1LX%2B26pWUkH8Jnx%2FleUBHuAJn87lgqdC5ScefOgwMAFaLdMteNODqMIB%2Bq8wdmNwsJf1lWGm1lVObb9LKx7w7KZy64IKv0q4cpOx1EDWrW&X-Amz-Signature=e2f982cf02b5ec14ad9bf7b261732c5dd23d7fe39535bf7ef145083b548ed05b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

