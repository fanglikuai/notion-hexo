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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZQL5FDMW%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T010046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJIMEYCIQDemhQnmDe4v4AmeOywA2h22WWtP2wOTt3%2BWo1jGqsZyAIhALMu%2BtF9XgxgZJSrWPGIY%2Blh9Oh7fk7nJplWLYtAan%2BOKv8DCCEQABoMNjM3NDIzMTgzODA1IgxYIHxC5nl%2Bed2zyMUq3AN2Ln5Rk3xlNm0ZelhaTZhVazEmn82BUdRo6M0bgThQ1E6Yz8wKiWf%2BPSiTW165h95PqQsC1qFE%2BvmvtAHILyuEm%2FJ1cGDyRkoTKfwHsfattVN1BJvfP53PvaEYZ3M4l8Kjv8prfllV6RCpljGoFdkfbDGuVwEoFIhtpBnxUrp9dLgzMtkMerF0RTnVulLPnnmLP9rEZcBqjC7PQPSUtsu7FQn9T1yja%2BoPR6eghdyYwoKZ%2BqcCHKAS2LHdTIsLz%2FsJLHkeXQpAJrWduIql%2FsF%2BIPiV7V9k4s%2BqK%2BpWaqKfDgqTXUcMG67H3QQ%2BBJfh%2FnNJmRsybNOo7obpsy7FlvjRDGOTeM95wGI4xNtHoCdYZeu58JvSYpp62uRyxMW2H32VOF%2FPJLU0dTmPtjvFdj4BTcTnbCxRhUt43xzLZHTQLMoMSIpWxfCfdcSbglJIsO18guSiVk8ljxil9nwacquvUAKcQkdm3RxlbIcfms9biv2wPpQMHYlYO%2BeDGPaVkerGBVWoJF1xh3gfo9Q4mpvNDk7cL2PEz7P5SsnZ3ejUBvlj3yzagh%2BkR8u6YG%2BhnX1vaDRJS3X30ipl1TTtmXrWLie2CL%2Bo%2FnM64YtFC7p%2Bhf4Oxb191Z%2FdEwcbgTD2tuDHBjqkAX2yUIxo7wMmbELu%2B36L8i9ey5lgILj5HRwRBoQX0Pz7%2FbFOzFLMLInAIO0wGVLGIBDMsT%2F1qwlr%2BGXA98JJCGUTQUUtQi5do5UsWXlXva6sKpvOBSRyT10SOLOgKKhVPCPwBxe2VABt%2FFDRLv3WaRs8dqAFf48phZgi6YeBAEW67VDhwgVHZLrAx1ucf%2BDl9zItDhGbALhtoK8FWw7XFnUin9TF&X-Amz-Signature=903846cd79aacd47bb3122a2548848c28f4656136bb546c426ae66730713e6dc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

