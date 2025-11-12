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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SBRTYT5B%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T170140Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCIQCbcwMomXWuzgLHtG%2FdYPnWzKWKWiP%2BDrwCPB34fwYhEQIgJq6jBRyYNfB9e0qMM2N%2B3DM4TjS91cbgYrEkhZKtIJkq%2FwMIOhAAGgw2Mzc0MjMxODM4MDUiDKB1g4%2B0LSl2DALojCrcA%2BLAoLNBMmfyGYO7NLurSH4PMmcSvD1%2BcRJmdu%2FtIcALUcScSBdOlyN3qYgv1o%2Bd24otZiAVD9TB7tpvTV%2BQiaZORHXWKTOBp89DCVhIRGF0hTBpVIf15Rp3UbIQaKiPyue9nWTGa5jadF33Nvz%2FxwYWFqoA87lRGRYfVegDADEaJ3PhKexx9JYeiGu5b%2BRxcwdS%2Fi%2FO%2FZEHfcoaJ2A1QPcjtdpIrl2AMyR7Em2egNKsZZGbtUEbiaxmEJkCXKdhmCvIyvqIBPOiUoFztwOo7S7BMJUPk8SI6xTU16qeD0v2MQomwjCiDHbj3aEry%2FkGZ%2BTUWdIFPg2axpbKPC0V%2BeWEloiJ0GhU2q%2F69BtBJgm%2FJPYHw%2B1HtDe2UdcpnqaD032Swt5ldKkykryAbOuMHNnXrua37vWtuAq3AgI9Nu1kEFGRK6YywqokF13E0K%2BDD0k4kwnmKSWEPKUzIXuA9P9ljtw01FUNNwlWmyz5xHPH2E05VoQDcQA0JV804%2F6LfkS%2F6AwmU3k%2FamAS%2BsqBLqkmFNBRPeosICKst6AZ%2BLYP6gxdR0Mytoukza4n6Uozc3dy3cYCz%2Bam0o3ckBOvdPnkbMuSgh0xp6i39ttczIPVoegXYEFsYCaTFrtCMID60sgGOqUBIH8ojRFhCHbQpYKdJXv35tUAzf0cOPVZ1gT8StCWg4apZlVqZdk55fFPSwmlS9zNoux7i7N%2BpKJvxsqhZ2xolrJiRgKmIIul%2Bk2cmpWsCRzoI6Fq1vEMo8ETSDpIeZ1W1hPOmZtdAF7ajPT%2FKQ9gvH3UUc4pxyeX5wy5%2Bcy6z%2BL1wzA3myyaAINBQ5lGnlY5TOlQkD3x%2F4mzCc%2BLeKxmguzyu2Vh&X-Amz-Signature=87f0a89a972222528a8354bac5e435049da37bdf10d16ad918b8ef3fb61026d6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

