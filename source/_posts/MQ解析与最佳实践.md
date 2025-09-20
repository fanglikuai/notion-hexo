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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ULLCZIET%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T230040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH8aCXVzLXdlc3QtMiJIMEYCIQD9PxMAcfILWRCOGHXfagzqSitYSftvq4jYRsOEQuh3fwIhAOHit2RpPUCpvR69skE%2Ffm2C%2FWRM1%2FxN0WB65vW26zwsKogECPf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwwQeERlCaD9u1wDNgq3ANqQrV6punJBHuVJFnqCdRnnxzDnFjnEbQjM906jtBTMJlwrxjSznn0Fa2c3qTyDS0Ad1%2BmaMB%2Fnsco9Rrco3dWfUymwRCXDnL5UNPzyk5Mk2uzcHvHLt1wopBbL0Nw3mi7aaTx6k6nYT6NvQ2imfUfb%2FCSOyA7AbE2pK7%2BU0Iuhdc%2FLA%2B1BlIEZ3S67q5zqJRkXCNUQ%2Fv7kVDw2V%2BnSFhTDl9kEkX50Y9dg8zEuMDtz6cAhrvQ0ZrqE6Lk4S1JN4eCVB8eXLtgtea%2FlHzb6gSOOIOWZsFoT9PFuGjbqO6AtyaEqxQZ7hrE73A2oewHYROzLWk4R03ydtuEUa1sHbJOAom2eO3Rfe%2FGo6gJevmpvOcw0EZBEorFahpPioCh%2FBYaM1zwx8qOIyeIdAjQBqUx0geq7TRb%2BT06ZiBUOrQQfqUyqDkNTn66rKaRUSEhTo%2FcGo%2BQyqAOvHcTUgs%2FOssHqqGLTqKRzkoSMfEUKSkB9vSIiVbjs49UlF%2Bcyn%2FC3FoSN1RWKQPVDCcZFxuNDTFxwrJSeLeXkFNJrNH2DgMPbNKBgBnuIq%2FKjX%2B86OBa87E4OYbjmHVHq0isFFzSnBTBzAcAyEA9gjC9Rsv4DqG%2F4%2FmB5buPAehMljvsdjCt0LzGBjqkAZTo45BCzYRZQv%2FJy3tj7DGxQLjD0rowUMy5QVsMvdYqxN8rr5%2FpOWy1qME7e5Qp04xoTAogJJ3a1sQsbDW2IwYEtNRrl9jfjMy6RoIGn9QuUNr1vh2GfoFRu8g5rJax813ZWTZYQBLBMorNvssWzCcRi9SKoRtm%2F%2FCd6sD9RBiV6IxvvVsrfUCSzyh6SADtRY1R340aJClpgpcfOzbA3OuubKUC&X-Amz-Signature=5b828403c58c07b5d33dbef88532f76d705ca394cbd4eaa1cceb1eb83bb6bad9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

