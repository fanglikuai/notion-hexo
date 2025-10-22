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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VYRQEJEP%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T090106Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJIMEYCIQDtMoZYJh6%2B8Xe20FQm3%2FitAmpPJYguYbAT7ygjJnVsXQIhAPx0FESnemgeSBOf21kE6Kj4oV%2BO8JnccA%2FfTZ2X7Q1vKv8DCCoQABoMNjM3NDIzMTgzODA1IgyLrmfv7O7UXy%2BqsTAq3AO%2FQngz6N%2FUe7fAu6AQdvvz3NIfjmztpQQdvyU5VMspBEZ5c0hX6KpHIHBApMcscDq%2Fny8Pcxen3ZFz%2BcLSXo6%2FuNbdiUOz%2F4dqcEIL9qDhhi3VTBELz63sYK7xC%2B1XoATz5SL7aXY27U3SndT075A%2BZiiDli3%2F%2B68OMA2ZDnpeyho7oivfpPmDpfoDnRKIctYqg4TYcb0VfkTBcCk46n7luwj6o55giZmYDrFBjlx99Nv8Vh9wkURVVT0SJpzqA8UQ4z2Lk%2F9QRPXUibCXfnnXg2Fpy94hsQ%2BhFc5xBgWuw%2BQiARRIwfUDU%2FXGoRnqcJIi%2FxxshS5LvxjYjZ%2BriKqittmHRnrM77lsKAwksfo4cX7BIvekzz%2FhwW%2FSKGwPeag40lFyzq4GFiaEBqJUk9%2FU%2F1dcZaonuwGWSlT792E9%2FM8VPmq3pk3OevUU2Yy7ZXxiUuWldU2AsdUf4RLnEqYs1NYwePuRMFzVnL%2B4IID%2F632S3kXqRefR8XfIX6XmToOFG5m%2FlNc%2FxBdfGioORkb6Sz2RBehQhfvMxijAvBkFHKqShGfsJGpHqs6UeUnhBYPSDod4%2BBWjEEkXasT9d3bcNuAQ3xtkskWzvVqVurFCqK%2F7gGL92PtKUMQgZzCfuOLHBjqkAVLzIPHLVlItXuhRDhy47EABnfn2wCQRjWTe9VLLIoonFjCU30jKm%2BSyj8%2B0E3HcUaVsXMdUPRCvqHDox7kYEgA1IdQ6zaCiogHtiDOJw1uFC78dywE6GKU50iMGVUyNQLOMJbtlVTBw60X4gBSuuOIWfLdsnkmjJ3qCAzRauQQHm7ae9thLKvnd1wpv1gapzeR6QsMn27oL3AIMZXL9acH2TPg0&X-Amz-Signature=74d60952b17bd905eccf64574fe98d81e4d667a43b380e00fcaa5e21fdf055e3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

