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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZBH7EYYK%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T160056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH8aCXVzLXdlc3QtMiJHMEUCIAdBhKHJoOMsZVRiiV8hg%2Fq12ML9RkgYUOsJNtamnYw4AiEAj2rrkZieE3Wm7Hjl3GQqT6AJfhp5Klk4lQUclIW2F60q%2FwMIGBAAGgw2Mzc0MjMxODM4MDUiDDXK0iotR8ZxDjzzuSrcA%2Fd4ri%2BXBikK%2FkF22nHUdkTa77a6d2mhMeuwVNGWxB7bnktm66JGMGJ0lFVW%2F7yku55ct5LIZuYJWQSWt6ysgXQIJ3kmFzu1VSIfAEh8oPvtmOWZP0e5AcloRM7efUWd9JAv4OokLl5be%2FnreGXs6fnjvguxS7beipjkv4snEN2V27f0QHGTv%2FF%2B0mqKa16K79BenboQmqcX%2F56AU3VwbDtdlmCjjXWn2eyr8iZAkhWMIe5iJeGaJ%2F9AL5KOv8o8RY%2FKkwJQs0dAtiYAN2d0PZ%2FTCHXNlmlnSF6SCRrvWOMRAc61xkSS%2BfG3ozN%2BGbWgEWAvfXR56aMuipdr%2BjXqapGGWGu9o0r5SEvQw6UblzYBh%2Fd%2FV30OJhhBLLs2Z1pdl3AIifZI92hQh0zOK90qWKEXAYTjR%2BCTsexaf4M5NIn%2B%2FPabcajpsDkZWzPhJEQngZPqCCYSQdYwmvRKthSRIvMkbHUVAcj3fiVpAT0Q3u6IkUdzcsvtI8iv%2FjSXOspXj64czl12LX1CHaxWiFVOb0L5wu6640i3kD%2FfTB8sy58uhLQkLpmzJDom253svOKA9NCfnp9GvM4gHsOrOUxherX8UqF0EmFBJYcjs3WbffKEtKVuluQ4bpzjTB2XMPj%2F9MYGOqUBdfe58p70IqMMbyMgtTFtBpjYyP3p4r43Lu4aMQKzV67tBTy2m%2BkQQhy1QbJkFYlQAYpYiL19EkY%2FweD%2BByuq6kiwK9lcDgliHHUVlMF%2BNWGWxIYciLyC0jmy0UUHO4wpT%2BmxTklVrYvc1%2FgXWgTwJeZVVm%2BfAxw2ojsghk1S69BVMyXw7fEHYTOpuT8GtuY8MtN9y%2FdAHuOe9Jbvx9VOWS%2BY3MFf&X-Amz-Signature=efdf863a012c60b0102a5240ea67c8f9630a31c69c4f8b14fe8aea7426c01b30&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

