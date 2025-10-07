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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663JRYWWOR%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T030041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAMaCXVzLXdlc3QtMiJHMEUCIHOhbPQ1dQ4JpyxJG7vIB1s6JBcRYIa%2BlP%2B4VRljj3bXAiEAv2gr1BKbOPo5%2BaizadYur9YX0gmsgmKd8yq8QRQiWCQqiAQInP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBiiqd40hw3cVoVfPyrcAxo%2FhsFxKS1U3ZLU8nuFN7qX92Ya6Befuj4Fa0jjNWnBDWtSpS8YiqEYTy1DIF5vN9z531m39brFu85G6MNJlsiXfbKiDsI%2FzXJgf9HD%2FzlJgXy8Qetsa99dkXJopL7i3Yw6mnaM%2BIzXpSrt7SrmzYxUTUsjvk85WlOI2HW%2BUXVieW%2FwdNUBUo5wSKHgx2IrEopl33ieT6RjQN42rHiKeQZuttNUCoV7BBFk1vHm%2BAN8sksilh02u%2BuiAUkaHRtL%2BJmw%2BpcPvBrzAG1VwrAH2bgKlue56%2F35xP%2BMxPrdoRDoqArFKVhWc58oJTYIOWNF4QWfJYOlXW5UlZJKv7venBXm8BYkNy0ara0J%2FjRx4REMBjHio8FMyJGMDwV4WQPZoakaH6omyrKSw3ejU3NKypisaWhpvkycivwvhxzSmuEeV5uBAjmwAH%2B4Mb7YoCKhVEmGVFSyGjw8ruRxD1hZFDf9MWmMRIupHO1p8jV24QHommXRxMFGTrFdOvVIbkaQmXk8LVn6XZPJJLh6jKtWw1syiFPhSRMgDRK612OSd7I%2BI964vcdm6XZckC%2FsLDtOyQUtMzZ0GdtQby%2BlPhMTwtXvgT%2FrrNFKX9NDOxSVYOkiohEt1S%2F7wmjB88cbMIr2kccGOqUBAlIJNP6hz%2BatKrHaAf6kpjimIfnZIybFgU3hFpb5Zci%2Bol5d6FvRpRzXzr5nCuodflz4CEMGlD0qDA%2BX9VEVjCI68DoaIbaT0KL9nzylrKnL3vt%2FOCxNQaK3WMu39vjvzNGNZYWIP8OYeqRr8BlyNQefvfjQOBL34P9tmuyfBHnh%2FFg7a%2FnL5ZkAruM89dWq6N1JSvR%2F4vlWDa7fBMQZGU1Ibyfk&X-Amz-Signature=ae23891af2f4add74d560e62d62cfdf81c9986cc035006b4b33697134d911868&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

