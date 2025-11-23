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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TN4AN4IC%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T070058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJHMEUCIQD7c3IWjRG0LjPmea7RZ7UsSTIGuiwPy%2FvESwSSQIL8vgIgM46sEsiDd%2FbSzWk7s1j8LIZ3e4LDjBbCPpaPXTXNfhsq%2FwMINRAAGgw2Mzc0MjMxODM4MDUiDDN09j%2FbjXod7SLELircAxSlLR%2FO2717GMYRWnc53UP7DmF0ItL88W%2FJG6dfR5xfUe6%2FMOPNiBN6gkE7e7hQfmCnRsGILH6%2FsAE1QDbwu0Fiit2bNumEULgVZWpndmkDWR4ElTb76WS%2FOKla9EFkk3XXGqeNQrQqQk3NK4z10tWMM9cmI634UISMHaFrKSUO%2BFW%2FDNVM1yFbup1uTS3Kre1qWR%2FLtatB5srh5NoozEgLlVRtEBkJzIDXSjOU5mU12S8GZUz9xzGojeRnPC%2BRy8CuMXwYsa%2BfNvD5xZ1ptUlf%2Bm4DmlP4ZJ5MmgqIi8qB4dV%2BnMAGPL%2FmJLPn%2FP9hwIxIQKx0WiWD9T4HiyZ%2FFbHvqP9mxsrvBmA68OUvbqCO9sMNB3SJ9vlcIoJ57GWcKGeIlf56VpeWdqFIQruqhtaTcjWM%2FFLBKYq4FzYP7J9h8hToiojft6KNoZa73KKrsgSuu12OeJC8gP%2BYAcnZq%2BbaAXFPpodTpzWCyYG75WzsM%2BbWWHjLGFyjeV7QAE4UExWWgpgheqrKw%2F%2BSMhSzhNRwqP77tLcq9G0f%2BbTVz9IdICbgdH%2BhKCL2aNsmGygB5T7n5ibmJFx7blxvYnIW2onngas6dQ3gOF%2B7sQ9UkaXp5B7pRIVvOd7QKNLOMMmPiskGOqUBRDHgvkfEaMXhdafXjs%2BYgF238y4OK5yZzsea%2FhTfqOoW63dyBT0ALWVpJy421ReupxVy4Nf9IO%2Bk%2FRw0X%2FSq6V9ISczTIvNEcQZv7kNCOjUJ%2BBqoS6Ex1oLWJwxJ5bPXtH8WlIq3Nek3R0mP7KjWhpbPV0w7vRmy4XRSb5tlqDYKpbHPWcaJOQyz1IvVezdxOtOLyub5xcu4CsnOVpi5pEQ5Sjf9&X-Amz-Signature=596046f4becdebf72e38ff9a002de22e28ea64df959b80c6e46efb1088a30e19&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

