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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YYXP6ONX%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T200049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEMaCXVzLXdlc3QtMiJIMEYCIQDGULARR5VkvPzgLwD17RCILATHJJIZn6SYdfBIxykypQIhAL3oswDUetNHK7N%2F6DfTUvqSvHDQt2DeO6Tjn3j10GlZKv8DCAwQABoMNjM3NDIzMTgzODA1IgzzA%2BFpOi%2BN6QcZj2Qq3AN7x5PsRcd0C4wN9e9JRrqgXVODcZjsS%2FVqvWNb9dxKdodRc1740ptBMkxhayE5f2TyBe7PbQZ%2Fn59KoX6U80uPAu9WMygd3fc1ByK36rH%2FWCqHkaqcdNt73VVwEno2Il%2FiArj7u7xqD8w4VdI9ZDEmxmQ9CR4EOuDkmQeRDfrx3yoUV4gwlU9y1lud%2B1SVmEJtQeni6HEpo0M2UsO6BLWEbKEGUsqo2x5m1tKzXZDL5bAdDH01gjvbf0NeHo1%2BB6IMnyPKHjsgAgpbHnPQS4IKc49J0E6yv4LhGcbYRSH04HhyY83IRN2ebFwHGcEZF8HjLch188KRhJnRr2lg5v%2BP%2B1jAVOoLE37qod6vFKngu7OS4NBn%2BecqbwZnSuu%2B5BC6kHiHSWlfNj7g8EO2moXEY0OMUB3SFkNhxF1T7iVVuYgL1XSFgzHaX1luySuYNVDZozowIyS2gf5boG6uQ4qoD5tfS2bSATQnQ42sBRHZZxqUwB3quZrQj4lJqFTznByEf7Kw9Xmannhyfr4UkYZNGtThANlFTXU%2FbHRsMc2aPB4kaZ%2FErAfGkaGHYvAzTbqDVRYtdcGQmOJFnlFcSvoy28L2utxD3a%2BW61AGUq4TFlfKW1aBA%2FUJ4SfY8jCd7sjIBjqkAVbT%2Fr8fcnzRh17JCU5GK7aMoqh%2BM0M1EvDoJpARTqKTPYh6qvwRF%2FubSL2Nb4ZCxshpQYBgm7CcvljusJaUlFTOfVDTpLbwWXOhviLHEqtnslpwivEG3PQuBojBdDNVip8PyIU7XpC83sHHT5dcRUnfi5W8ZY1jLM0qVFcL8CSJedzFn0AnB0GD50ReJmjydWDogBB05S0XOex1bVUNscWcqd%2Ff&X-Amz-Signature=3d6768d667c2fbf283dd3f2177c0c7a5731eec09c8968e204d24ca528c75672d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

