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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SLLL4NGC%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T200040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAMaCXVzLXdlc3QtMiJIMEYCIQCSMnS7ELCinKn7uSujjF0vsXWsBPX3ZPswbFOKETQFRQIhAL06DHsUEShARR%2BOOWosw60KYCKaDRcIC8yftIgyak4JKogECMz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igzyz%2FP2OmUGhr43zeUq3ANL4RpHvq98Xre%2F%2BW21gzDgyH2iR8JO%2BCz%2BNDwUkIHndWDRRgyeq%2BVtgadcNldZPSkFLWW8rDY31JRCJoc%2BRNO3syyiP7GAUCq4xjLvydh1HN8UO0%2BXytLZV6VkIWpbQM0%2B1cb1jB75vJ2%2FsgeYoUefcCQWpef%2BTqFK6h7HURexgQeW6K6%2BsScS0amoChYna1CHuVDMwZEdtq%2FEl%2FN0dh1EDmBdje8fb%2B9y5JmNxn%2BvcCOkyCw4T0ZvqDPLthJY3%2Bch%2B2iVcmsn0zVMySL74oJxzqm1hWkmerbtQNxCPu3r3BnB65jLzmvWMiCbhMpKC9Oro67en%2FsfNI2lHPlV0BINP0kV3KR1kpLoq7zm1pSc0M6fw1EkOhigz5PvlnJSFkWBmo%2Fk9la8KqmgWWRvz%2FumBIiZ2XQ%2FIf6xbR%2FSE13EJKVZ00i%2FiXdJawySyiGk5vBVBQfL39jw%2BvtJ7ArcMOqzYVAud3v4whFT0hX1tBQCvrtKpYI%2BqtVyI8LFnwj5dJhezm38r9b98IwPtBPz%2FkekL9V31r1Icr6w931jumPlzyNZjdJtuu13I7337Z9y%2FU5iIFL%2BuTjN%2FLd91Dygyh9xg8xtSmP0a6F4gDNJ9oShOVNxVzHYT2CjdPfy4TCmiPPIBjqkAfgb%2FEZEdqOsh7VqKhCoZNe295lT0tr9WlpIsHAm0b2u%2B66K35WFIf7rDW6YOkhjsCY0Iadq%2BKU0eMGq%2FYzro4khz58zdBoBGB98nehBzz1HbcNon2O5R3kku3pEN5a0yMvMaBVBn6Akj9cG8%2Bze9JybIJBgWYE%2FwbYb5O%2FAxye0fk%2BEl0p%2F85kKQOThp%2F0of1nG91loAX8sagbfviPXnKI6JZBF&X-Amz-Signature=9b5ce1500bd05cdeca59bc7d09957250513782a20f8eacc954e45a11a3b6bf18&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

