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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TAGJTY2W%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T230039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJIMEYCIQDJhoKLWYG2ltvaIsrgoQt4bftJaIWWnaJkPd018bYB%2FgIhAOCQ%2FjQqONTY2d65x%2FI3c%2BA1kYWJO8hm%2BJB9D6lqsJI3KogECMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwEb%2Fg%2Fk%2FAQX6Qg0T0q3AOcW5IdlGl%2F3fOOkqVeROG6xXmT3R5YJn4jdf95gvJhf3s0CLMTSM5x98bJ2UELQYp%2BtcGYb8mHjDC9F1zD5EN9l1pBfXrhx9hcFw5OX25OrpuBdwToe9YtaaWNBozAM32ZxwKpkpXMjHK42Gtq55YPyeTkML0q%2FPniRqCuK8XFfiUwIXqMPxn7uR7ia7kYhc0gzsUzChXcBAke7VF0MVlI5W55cFqRT0w1dWdY%2Bq%2FlzXj2HC2%2BU2DJ9%2FmzcB9auvD0HpyQko6E5Zu8ccb%2Bucayp5yz4kFH2EkTFRbUFSyVZlf7acfOeCXFGwDRFX6x5ztw6FbDD7mxRjY1GfhCjEqhaHfuScZ5dO86soOt5g0LGPBmaymcqzgbMETc2V03MfDzJQROf2nW0Vhycw7Y5OOdTVP8il%2Bf7mT%2B2zvhb5lD7QbN0nNdIBjtjmOd0nivXij2t%2Bvg87l%2Fl0lIXpPv2divK3S6kkDzZez9p1z6%2BtIGg6uRYwXRUhtCb8cE%2F4EKQ%2FkREqr6uWtx6zdGyNu6nbnjDOz9xqM5mTBEVBMX8tJ3eOx%2FO70J5oJSe%2B9PxJTy6nEVnSHz0RGZ%2BJUFj2OosYyFIgecvVph6gniyuswtJkVnZyHeT0sn%2FO91%2F84eDCgnLLGBjqkAWlxToYSnNJXO2oF8DTEy9aWcowillFYQ4eaxJTribWGwPSfG4nGLlxVigTaHOIg512aQezng0Z9GXRfslM%2FwnUHHsCRnp74DtTDj4sfwitW3UdlGB2pzfk3cETJnK2EuOVAHmeQk4hbQw70MispW0gQkF4hyA3Dw7AQBpDYOTueYo98F9bzJm0GQZi7xOi5WAUgdeuQCfLSnjE0YfsEsfOdrip%2F&X-Amz-Signature=e5571a4d601060edc80055de1253c09fefa7907b812a1bf9528a8eb70639ebce&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

