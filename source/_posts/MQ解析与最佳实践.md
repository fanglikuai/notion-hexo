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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SZYUYVYK%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T030046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC%2BlVIaLBoQtzeThaYr3JQIHXM0Y0LmgtE9a2FDTQZUvwIhAPfQvS4UcQNYoGd8RhrrNE682vgQemhhO8BZTGQJSopiKv8DCGgQABoMNjM3NDIzMTgzODA1IgyT8KIt%2FtyXGnrSclEq3AMKXCAjEnxatBBMrrWjmcIGh%2BMsH8nZiW1IRtQn%2FSGrNS4nnRe2i2pD3WLg%2BqURRenZl2En2KPycCpbQQ%2F7ebVbTDo%2FSrXl8O0zn34xtvtKSqozx7PlL3KFLKLql3maBt8N%2BXZPkAfCHVT%2BEh8BqHJcNHAYmxiBvVMriBeV6rfHPwG1%2BWn6S2ZwUYskT%2Fl2Jhq8EJNbwLSo7cMMAgUMWiLF%2FT3%2FhYBgAInOuZQq23tgaPJmZGlEPHTjLo6kNY6Dppo3ZMPbyRc%2FaOqtTACRBhggZ5LV61T%2B7rBqYXhJ4xir10CNE0mvmAzuzDw64oK3K1EM3iblugoAPFpuwP3FC9yUoK589QeL6uQp7joQI%2FxzzcDuHLc13b5PyK0nMCrqcPDpf6OuRCrTm9GDH1mXIw%2BxUL3TPxmgUIwK1Kz9U5K4vwiy2tcFXy8nTbxvhRTOrRQf0VBhHlGnX8Ula5ObZgdc4Co0REBCSrV475HXgPzHmREXTkJg60draEz2x3Fq2aIAd9vl8Fx5F6JjuKNzf%2FmLl3RaIQEulwuiv8gUd2AYQSHVZL3KS7IRwBilj3pfuP6seu1zfwLFH%2BbAuuVQqU4wC56YS%2BAn2zWLYK8ClDebQhafTs2d8kkHeuhC3TDG6NHGBjqkAf7M3SSgsmvc7DfP7JD4JLMBUReGx9%2FbrQlMZ14%2FI4r3DzzxQ0X%2BxUoAw6EXRRXU%2FWJfqrGg44ZIGc8S7mqye6nfrZE4x3hkPhuQVRSZuRDfvoeTbi1G%2B2NOEKefMlSa3Apf%2FGRhRvXmHFeUWMSFhwO6hjgtUkcfASl7lEkFjZYECJiBcVi20ATy7HOjWRpQ9XQBGpZ5DSn3IDqNEH4eDWMF7WQ8&X-Amz-Signature=2faae4a5f8161c49731eb4d43a2bcf9fcf6d7fdfe288f359f4c630735218cc48&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

