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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665MCAG6TD%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T170038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEID%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBQhJTuhuqDmw25kk5CmrqfrBxIqLNMxVA7a5ktIU72BAiEAhBT6UJDF0eLSLWbz1wSMgIAPoh%2BBAIkYsHDV2jfmnp8q%2FwMISRAAGgw2Mzc0MjMxODM4MDUiDAygsP4%2FkpCGvWPplircAyk3s5pKtVg3f4IgXKAJMVWEcrURv%2B%2FyT%2Bp6WZIxkycwGPRrnYUu%2Bme4L6vDcGWgOMOkbxmMbRisfeZK4g9W37ScD%2BBhdnvO8CJrHx3RdFQQ6eHXySPHtmBd5tFnkUAn0c4%2Bk%2BDJlgx6RNH9gTdypU%2FNHDFwq4mQexvaZoyCeY1d7ii0Hp2M2S5zhM1J2jIxE0wWy8vdEDBvhVoXJuKWWSjwcyXVC4xMGWRYpneKhQO1SepsOBBeVaM%2FCEEcZpvBZBQ2ChO7pHFOtpkETFUc0BGdNJTSGk7y0aJQI5bWp4LGEpkRKwL%2FSfM0ZP5XAZ0iXDucu4PnyBuw6vNkGtCw6RrA9Zh4nFJFL2fmaWwLxzd5P1AYnWodPLoNPO6BsRQ1VRqJWDiTVgh7SHaO6LsbY3AieVP228EFEdenRtgLjJpzyyI8rlK9q9l3mH43rjy1jP0AfBHSdHPHXZ1oIMKmjk3w232ZGZu1XLYPMj6d%2F90kJBvV1VrXeJ%2BsKYu01kolrwjtJ9iEiyT01Ch9XcCPeoDHG9mWUl6LKQ%2Bc0AKnClUU%2Bhtq6%2B7og2gS%2FTwgBswu92h5aNl%2B1RSDNOQhaRJ%2BnhtUKq%2F5dLtjaevzOTDvA7azsEJ047zkgnXmufC0MN2DnsgGOqUB7u%2FYkGzJJ0RBjeO%2B86PvP2808SJS5GZy23HMmL0hMY0gtVXF%2BIL%2BwKbzA0prG2ysNAda5MMiiFEKWuAXO0gZ44%2Be7v36yOgF81uTgfJ9TKtsi4om3nqJssre4zQ00O8DgQAD8gxJ%2FaEHlkgC6W3AG3E%2B9%2BE%2B71uypUlTgr2MkprMQs%2BhKIklPrNGnWDycckDhz5mdFsukAZfo5yHVBMn7bj3M9vi&X-Amz-Signature=c7e94ebbbe16c2b655156108e2bd778eb0132f61acc431abca86082de8613553&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

