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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665STVDGUS%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T100040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGEaCXVzLXdlc3QtMiJHMEUCIQDl0TP2WRt87UTFmdQZUNPdSNMFiZjWP9XBLlYxIpcxvgIgOCEOWjNbbO0srxI2TzoW6ecLYbbil3gfWhtIFEE20a0qiAQI6v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBMtQxSz4ZyR2u%2F%2FmCrcA%2BaHjBFMuHo3AHsjVGPvf%2FlZxRF6JunZwwVxOXPIYhTcDZgoMTi1DWVscl5fTgStlvhHShJKbvpL%2B6U9Y4R9%2Fn4cKr6bmIeJw3SA4S1I2LL%2BvBQlj5GwJv0Nf%2BgPMeewHxxh8j%2FVtqp1BTR%2F2yn7zTv%2Ff2VQk9lV0FWBBQSzFjfK0T9jsSmLfJe1KoqrLxPsR45ajFd7zuJe6cXEgzCcSRUHyQ0KSAI3K8Fy5bzOMs8HcPBIYI7kaOsWu9zVOiq2avcoQsKiBd03%2FHuw5Yk8HRYwi2UCmqEUzhDHNY4rSaP7yxU7%2FVTTy2UD6nvSXTTsAkrtN8WfDVNQI6b4znMb%2Fa2D3RZJENkYWVyq7MyLp47tqu7BJAc4ONheJ793teTntqdz5AJA7k5djcGVdmobA5%2B1y8XEloxyyrNQ%2FoR0Zkl9wtI4t6zHNkRIsQ7aV4TAlrA5xN99fmceKW2GXe4JhcpvsQwQMAfm9b4JnWHfIfzdyleQaOQ%2FC6Y2VHMVug%2FHlqAXzsugxE5ONgvyq6CeMJzdxHgMz8TrnvRzGWW3izHvmFCmBtOtXZD5bribHpuN06Qhk4TQlccq6gFkbycJ1TNW2P%2B4rvQf2dgtiXuf%2F3UCHPkKl73ADewdJzOMMMW%2F7sYGOqUBpfnSTt8xPRzf2U7catW6xacb%2BkLFSmvzCCO5wPtmJ7mJBEndq0TgRg8qtd757Vnk8L37ujnqSHSf667HPkRU0odHRgTc7H36%2Bt8bIz0oRujdkYqj79bdLuWEyH3aACHOX5jgCbr6KQB%2BbAsJg4MpFOfCbFiIkTWFC347D%2BKY8AP9XgJZqgmuIAXyTrggNjAhM0HzBtamnA5SH%2F8AX8EkIR5Y8FQq&X-Amz-Signature=0273565c5d6a20ef455f689ca44212d65eb0ae26ebd1e9ae844253b150eaff36&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

