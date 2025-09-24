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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TM2EOCV3%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T120046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDXrBBN19h%2Fg1U6SNpOA%2B0%2F6i1VZAzrbdZ2fYvhq1ahngIgD8w0Wlll9YQijW%2BURp1S4eV6lILKU7Bg4UhhdZfV770q%2FwMIXRAAGgw2Mzc0MjMxODM4MDUiDFRkm0CIRvXN0OhIvircA6hyNn92wizzhK4WfpzNo4ax5UeIuUtI15ZXEOZIRWc4%2BUgO%2FNWZI%2BBGvDMWgqIUFgHDxg2SPGLekklovoDO2ABKR0uPscapulPGvCoQtEEL0nodD0nvJESRwD8FSW%2BiB1WNBt%2FPz0klcaEOfcrsOBnULrYnwiJ2cra1LZ4TnIy%2FS3u9PTCTCFud%2FyfeQVsXgcnOIM0AMyVs1WC3GHFgEQnKOCk59xZzOc5MuSnjd1e2xU9QVApKcr9j3dmTaNDGkSPZSWr9NwyZZR3BxlVjtZhwLt8OISOBi2ui0bCYQKXlNtvmcVyVdy1JiqVSs0%2Fr3B1F6zfkRFp%2FhFewos1kOJR4%2F0gNZCFETcl%2BYELataxBtRfw2DxmD7uCVv2EUpYIuSQKYT8%2Fzf8bzKeF1aadhJElvdOAh%2FOYLiZhUuXItUPmVBJuhso%2BUYdSHDJgTyBGRJR0nw4RxlM9ffRY93B6EWIFeIFDLO0hVQhq4JtfdymJDxYcv1IhO0eyD70%2B%2B7mPZwKl4IfWnUgW3Dg40uWYdgrnpBgIuCVb3%2BypCJQA82%2FTQWFK9onMoJ2kp%2Figx%2FOQPeyMLtWTOBc%2BOfjZaUfwu2ynA5NYa9gN1GoTcFx%2FwcnoouGdYKwTWcijYm7GMLyzz8YGOqUBRJfeJedOXpHp1puwIYAzRmZWETl5DrdCn%2BXbpkDgVqkB7nQeL%2F2uzVnK%2FEgPUzo%2FFoguKbZ%2B0cUpr4VplCNHcxC%2BVd9ogzyKiTwXj70zsaQZVV4AVC%2FAVaADhzPbZ7lZbuOAJEe7xf3eSRd%2F7dQ7FWOfT1cu2jUvnIsY70Xgn85neUXojrCE%2BOrdwaCOT3OapmyyfO1RwlRYtpWqeKMDvMPOLRDD&X-Amz-Signature=180a5e0545c41af39350aec610f213ca7fa7c8f041ff79c51b9537261bbf657c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

