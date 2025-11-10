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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q4RBBRUZ%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T190046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEIaCXVzLXdlc3QtMiJHMEUCIQD6hOwCggeHD5w7NuNthLpNoxs%2BfS0sKhVG3051etKdmAIgDwsc%2BDVUUWU6WkvUdfy9k5%2B8%2FAJheag0FSnWGOtP7s8q%2FwMICxAAGgw2Mzc0MjMxODM4MDUiDGzHC7JZW%2BPpF2GVwyrcA609%2FIHP2h%2B3heGhDlyQVIEh4lCCGn3AmVMmWDDmZfoB%2B8VEpEiyJay8Oa0eM8i13gI4YmthVPmJEf0lOeb19uGqimFO%2BVtMtR4XeHWi8qcDhV%2FpJbYOYYLfE5F4rYuGlw6scevCmD27QqYuHRIpXBiQu%2BxrpCphTrfR4KGACa6aajp9ezOCwpksVaiIqOvpKkBDWcEKf%2B51pVdwOy5EF1kfZ%2Fa6pC3nclqRJ73iXLcCH9fLTo%2Broq5UTTB6anxvMgxDX4lY9wB4ZdkX1HQPep92ydnXJU7xOqo5VWDw5goIpBusbiZutMrVGndAIkhvrKr616bjxWIERQ8JS0hsVsHvmVb1iMt8%2FYYZqfEdJM3BQ4U3FJMZ7d7%2FLn5opSfz7n0iaqi570EY2GCnkYR1xsCF25OHM9vYWN9juftxILqCbORTEN%2FKccxrmHBi0aMnHyLhDR8yq%2FHxg7eImDueBFAIdxVvzB%2FhORQeiavhg8iA25TTM7VuZ5azR0Vdn4MqQiG6fDRGzhZsP5a0PhB%2FN0RyAt9r2%2B56%2F1HiTZ2HJJeMteU97BS3QPwZd50sE6hA4xD5oM1tEFeRUtKCcYccF66UVEZwnzfBLNv9pVAoihDEpQV7ckrps0XOrNxWMObLyMgGOqUBS9vv9B1ua6penmmEidaw3BqBVwc4VrhqyldZtRkwsURCuWYuOG0RZfB%2FBKXl4lsuIB%2Few4cJuQURkPL67qYSVQW6g47%2B5giawNEUjkhwJWM%2FVVpX6g5SrvBvUs69P%2FY1%2Ftv2259huyzKIZrB4h3XyUaNbE7pYHXmHse6tCqX3ZTSTnd5vWWOJkZo3LxpHIimQdokLOTOYAAI6ArazbQTmsHaptoY&X-Amz-Signature=4821f7d052e6d05f4d7645cd6feac5a23000d62f108c74bfa318f19010c7a170&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

