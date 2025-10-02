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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RMVT3CQP%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T180049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDZRCS2QcCibxM%2FM8TckEXgkfis%2BGOZrBTAYlWHuAiypgIgZ%2FYja4CiMTzzTBD1ncckbXairHo3ETCa8uqKI8nu%2FWUq%2FwMIMxAAGgw2Mzc0MjMxODM4MDUiDI4t%2B%2BUaQQ3ZrtuzFCrcA0oqY56ZpJFzjWtQqdtgxEuCwSXEvoiJLZqlh%2BUqCe4V84CcNjoCPKI%2FYvkqtO5J6EaXiMJGFEB%2B6gCSdMkw90djCbSVlHS%2BdC4TnDUt5FYDpGhWNxvRwx9FHyraLmNdnquab1yH4LbOBxuFRqoW%2FjgGFW6LCJzbUI%2FYCPHfmgrmthTK%2BQhuRM3vHA0m6WX1uwH%2FpGoZQ5%2BI4tTOGDc40d%2FNIkcXTay%2Bx5MOYfhbBSSMK2OfQcBrJrVwqydibVKse6LBh9NYzj50mzscS0yuOnVU%2F%2F1MD8hNOLDV6%2BbhU7UuxqIu0UTQeW09Ji6z3ctV34VA8Bw6BWffEsJsGFoUqQhW%2FQT13wU23oEN6vu%2FF8EP26YK66a4G03Oz2vup7aHcpVNkSQvWHgkFaLWMYTG%2FJuzJ4sD8C1ZYlwwTOAtwGu3mALHidAW546zR1cDPidNI7AxyzDJCuIF%2BKBdCuaZXqDky8q%2BrjzDdQB7sFnzKPPhGJdUVZI20gYFdu7zlLfoprjY6%2B1MV47YW3fKfix4HV3RC7EwJRj93RPMGRSXH1%2B1T5G%2F3rhB0p7QiF48coYyEzhnIa3AMIN337vCcydyz9DbdQhEy9FugQCktqSEk6gRDis8gyieKBKeTjH%2FMMnz%2BsYGOqUBvvnzByWReODJ%2FV6PsDv6VP%2FnrLuVp2Z4VCRq%2Fq1sEVeq%2FBFkqH6lQpUtdrrweA%2FsF%2FHYxIynn4NYamcCAOm%2BrNeWN%2FF4zHDCc%2F3383DHUm%2F%2B2M%2BglEQ5oty9E8KAMfxQHgB6by0XiZIhjWd3C5Z%2BYd%2FQL2pUyZMNg%2B%2B%2FvRsZ%2FFRkBy2ObqwnHkE9uC2w5Sp3AFCZUNUQFsbbwgl7KkVVS1aiI%2FHZ&X-Amz-Signature=f4f131fce0c6dd9d7aaea589edbea3bdb57cf5b617f3792d3a40666696a34b07&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

