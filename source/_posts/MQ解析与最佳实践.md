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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YDIW47HF%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T010041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGkaCXVzLXdlc3QtMiJIMEYCIQDtFgSuiLMUozH%2F6ptq6WgdjvPe4chgl1KRIKkU0KFqYgIhAP0BoDDH5%2BKK4KhqybXBPH3Lv1Hztuw9%2BOEWPtL7ZZJdKogECOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwtDDTwe%2F%2F%2BNes0hzUq3AMNAAwMwjGxAfX63YEe7E%2Bqf%2FmbXvXNJIOP6wQbbh94HcF4ukgLTsJ942peP9CaveeV28VQiM6HiU0z32VblJJuZ%2Fv8qCtv0U7v8BDZEKWm%2F16sIROJ1woYUzy96mNHAYICoKXqFPdU%2F3gRlTtijDxIn7bSj1j%2B6P1KS2KCkxpPo6A2NUCofDBV7oPhk4%2F%2Bo2EwsSaWm7vlHjIc2Wo6G8b1CzbuqVYqAc1WUUpnEp%2FeCqo%2B7f%2Buouw4i5hWmdefBHUSRRHDgYGc9BwNAdzeVMyWRG8dr5a7AnIimSzZYl%2FEYbGPIbwA9nFDt4qMT6GEDldPicsazAj4fg%2FqW7CC%2Flid%2BwOAtNZJvPx9tJwvTUhhXI1Gfw%2BTztoZPY7SRRigygIKvHhNrBCyc1vdchHFk0tEFkCX5nxDFr4%2BvzcAe6Yh8ez6K8sVrAm7VoNwkfGY%2BppOxGgIALGBuAdFQzL79tXqWQcsY98aDISP1HQSfXXJ8rcIKEqdg9H82CoL7pNHEN1h5crZ7WFBQ8swUHaLODjWD42D0y580PkTsEaCVdl%2BHHek5bbQPu11vGBdxOvnXVD1SkEHtxc%2B%2Ba2SL1Oj7F0E7NV9wJJAkDB3xld62C2S0AeeUe%2BIRZ%2BxtthaojC27LfGBjqkAQVvPfHensH9%2FQiddxNdqdtKYNR%2BE7iG8D9JeYDEfNqUsL0FC3FDSwA3KWvbEnHu0t5W31W1yyAckLVH6dwEp%2BzZqcjhD6khYyZIwAkq96VuD1huKZ50YDELlOU6HCdbr%2BQQuCmzeUrUSVl5K5OZ9ABt3BRL3BjGwiyXQaPbULxeJx6%2Boj3YNJt5c82%2FAZmaEL%2FxTKRHMnXFkH9MAb80w0CIihEQ&X-Amz-Signature=a3093316ec3cba0f4e551408e01475586293eff1c889440ea9a64c1f6d8661d7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

