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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TVQD262F%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T060049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDz9pO0amwxpZD0ErKlXdSufwFz3GkWxwXkd5f%2FGYw0eQIhAJsrJE3HiWFnyLfMz41XzJLZ980YN9xW2T1wWXVnbjVCKogECJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyBngntsbF7JJMNbHQq3APctIwj2L%2F%2BUCktbRO4tQqzjrKP9t0bhPPGjS6YJs%2F77PObf%2BrLnuoV5RsZaZ%2FH%2BV9DWRxUPrfpywH%2Bd5G4wghE6lw%2FX4L1ffJq1AjdIQf23JSw8QXbfSa%2FbyFqm3ah%2B64Ed0gcwb9YAX32TJ6d8KIhjdF220sYY9a1ukLs9XoK%2B%2FoEW77pcCbBeSt0IjBz%2FGxrmJfAjIDmhOGweYJewfwYodTJ%2BQrqsMSPhBakxzkLzSIMJFOgHhWd09MHRR40ncobH7sBJPIjHCRO7LxtRNnWna%2FbDi9rliM4whmOdC%2BA6qlMTBysBxunVQrKRa5oGfcsevmU%2F6iME1FCT93mSvSRBHtRaMhzItgCB1dR80AH%2F%2BDnmVZ2vSZuA29iJSWVLsgVckqgdBzFb52WFleMyv%2B24k51LPuP3qZlcPHOhSg9plkoWHYTsma%2F01rN3UYs4LqaZWqzf2zqe3cGjiJlz%2Be9mbgRIaXQquCDkVty1NpfH9keERGdaErqMk%2Fk%2BtuxHGeXQChSRf59iFn%2B2xAUqvI21Yp8Q5fFcYmmyH%2BroYUkeOAVBmqb6sgVSCgW%2FZHSCqHRdmZ2LKi1XGy%2FwmsTY2tYc1jZnD%2FyJVXjMFHx3cZmJgMm39cV7twmvqG3KzDcuJ7JBjqkAXgypXtXSdSZ1t6DKVSCeK%2FPNP0JVwenBAkxRbtLxu9J2u0AzWIQy7n5XzOz8RHLryYeg%2Brc3eJ8BHtqLwHpbR44a2PMbvjZ81cloR%2FX9esnXqaGQHoEnYdJwqW%2BCHRvCKW1yFJLkWZPPmIVP7pv%2FciDuCDT2BVl2YHvSrvUWShjBJTGh7f8NTluJiZoYxh%2FAFN8YJj79M9TxWl3IKbwfv4HtWdC&X-Amz-Signature=deb14a7c347fa60ee5c67b533fd837b906cd27be4318317dbe5c2644c9e7e86d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

