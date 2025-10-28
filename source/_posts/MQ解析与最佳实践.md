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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RG7ZE6LT%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T050045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDMvqZeBpApyOUhgRIwAvQzlB93TyvbnfXSDzzcwImiNgIgVbLEvyAD1Liydz3zewk1qAPDDJowHNWli9I89zx6X6kqiAQItv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHcJvSNyDObKklb8bCrcA07T7HBVdsmYFe1Rqo%2Fsk%2BFNGGHRuHnNfPFtMaMVcqYXW9%2BTMKFAcJyxVOcoHSchUN7UAlKvibCbu87sINWqGleIuzKc8RJLS%2B7Yadprk9ewkhRR%2FzFoZkOqqhqcd7iRZXwRRzoyH5jfhQurclYt6c2Tz0H1TkpnblxArG454RT%2BlvyMCkJ%2FVdrvwzEcBWvUDG78HT9hDBGjoASv3j1pyhbotADr8vCWtsyrhykEQOCuaV37NJz3F1Ews8bqrwEN7g9PhVIzaD80yRoJZlv2N9emPWGwMGP%2Bjf6HPunrHFBUykzMjJVoO5UEscouU8lkkbWBRtZxUEMLVnG1Ug8PXTZ%2F81wEwcZkLYaceBCB%2FSuAzMYx%2Fm0o2F4K%2BVPkvfCIDJC%2FkcyM9pHjpZs2GXQn3HUEwCXUPX9lwJLjnacz1admPzz3VwFkbhCZKVCCTCkAFq7oFfkRJsoPk4fwJ24iHDZD7njrzWqfLm15RSigRhZZ0V1eS9dEWGKs9H3X3wwigT5cqKUnj5%2FNTHKRuLMHbokIBu400kJ0AqOQsl1%2FkURhiM8CORNSG2qnmkcP9xRhUe5B9VxBywYZ8yOBMLv%2BR%2Bmnr5S2K3LeS5RtFT%2B1ZrkfXSHKBL8VSUlbg16WMIONgcgGOqUBA91r7ehgrWsIjUACo%2B5YWMTJIygV8XyUz0Y5RJAg%2Bgm4wOw6gCytvMtFXIDPotrS9jsqbHNL9O47lCHRA1Dl4421zrHxSDuiVTcgPhTmwMl98bmJkvu%2FOAtDYQ%2BjSxN4LtjuYhq3DBuHujvTorGoGhs0CDt7EK8IeSjMG6hGIDH8fEz%2Fe4UTEY67%2BxOBZtk2Piuczl6qZ1UQ6QTTHVr4wq8%2BREm0&X-Amz-Signature=4f20a83f58a978ea37ff630eb6ae9649cdf407b6d70c6cdd6df55a1e4419e2bf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

