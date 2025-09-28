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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SI6KBR2N%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T070040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECoaCXVzLXdlc3QtMiJHMEUCIGmgbueBVh3QgH%2BpU4tN70BDdlnCxpJ3JQNekWh0j7YuAiEAio8SZrVxJUrIyWT9LTmfOEsfIVZrI0taFZCRfTIimkUqiAQIsv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPrUIgtpxABY0gfZkSrcA6402w1BClLfo0NTobTBWrlSSdvsMk2lISUwZB8K69llalWvypmTZPfQotJGAto%2FlO%2FqeJqUd8BVFlNGES23umVjjyj4RBC1mTJJEJc7JojG2AHKxdn7IlCvdReQhvQKDQV%2FAud9VlpDH8wH574VfZ5gr2dCClRVSjB1mpPkgTeNkR5oWAR32vkS6Q4ycodVxywhDMINc%2FVgZMKsTedPSXfJjC41bfxlrxy5HLZKhtoLJoTbZ1N6jsoT16dDQ3zRob5D1zYH9nAESRBzwTDrM%2BmMqBbaHon%2FPX7NwSEaO7xbZgY5k%2Buhd0pV9FVlgBOeRYPVrjvt0XkhdcTKDSSWgMbpjgZiP%2BV06uAZqYoj7Z3CNapUfSa193G6oZiPlr6Jfyh4avFeS9kLdfx3%2B2%2B%2FN4jkORl2VC5I37CNA0gVmjQZlXOm%2FJKNFGJyDfKJCb8cLMbo%2BqsTVJ1%2BpWNCJYTi1zaAW5GuDEWgrFN62xNnHVCDia0Xwn5CsGH46aGKy6QN04kLHiXXNyLpCEQH7XVIl8a0c6E560%2FTz1lGrfImombBIN6GseaQD8cqU1%2Brd6eSYvNEyMkEzA%2BW6maJQj8Bpw3g2OUVHuXrqOtdSUCo9DS7%2BkQWqcp%2BENZnDtahMNea4sYGOqUBOQ5Os%2BuSMOpMd9%2Ff5%2BsDKD9ogB0R3I0mkDP0He5oxyikzG0e39zGFVCWvgFQNDH3ZwuRlIWPGnuX4tCX9e%2Bk98aOjwgCQYtJaaCF%2BjRXxT%2Bv54RTBgOF8MH%2F4n%2FY4tUXNe7GGvoSGn%2Bfl05%2Bx3HJgciNUOL6sAanfX7SJ7MvpkE8OufkPGPqHeue%2B86qPUIJ5ykSJJFzKN2KmA2iWbzneG6SztQs&X-Amz-Signature=65a68172ac24b6f83c08439fb22045d58ab12c01bae6eeaca70c8dea896cb6a1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

