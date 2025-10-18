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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665NHHQOTI%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T060050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA0aCXVzLXdlc3QtMiJHMEUCIQDk1590Uw8ivID3U4wWQXXl0Gz4qIhYnaOA4Je5MFWH5QIgF85R5YhE5tlhapXbY%2FJKVDIUMLRUWoHvngoE%2FmRO28cqiAQItv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDIxykdaP7e2VQgwACrcA4caMN5V%2BLoGRhJVxcEN2F78CG1AV%2FlYkO%2FVS4rtOEzebYwJP8zBeHVBY0t5vpuphg2qq4O97XdMzShN99dBt7da6rmC0XrqBrBw3haaoRLpiXzXEYhLNWomCm22RZ6B%2B75uvW98iQUHEseIwwjvC4jw7yQqJH5nCOQmcpTdC4jdslwuo2lIfmfaz0fphU2TQdj2PHFA2gt5V0S8AIiFDTjP46Ka5dRtgyhxIdXL3mF6W5v%2FtC0XAaR28PCaLsAAsrUvJsYIilzhEnntu3uASvjtNSYhBXTECH6V58WOR9RtsB98cnptZYHFqw3tTj15l5A95yyL25ELy5gx2z%2FoSvb7647rulWRUeWtDCxnsMt1Q4M5xigYZuuLYMHVKiRrb%2BZZXtxm19gyJwKNhQ5R8%2Fs3%2F0Vs9D9IJtDW1%2Bm0dre6BBHRqlHcVpeZxdOyv03UZ%2BKPG59i%2FPl0kBrPXRarpdeaQVUtmBRjONIgBqWXw4%2BEqew3yHlDajlU4wS1tGxErfb0B%2F%2FBUlOC5TgjfIOLepXJpt7lM15kB76ZoKmdjJkJFlUx5PVi6Avky5HzHWmmLTHxvqkSIHjy3q36BGX0HmqHd%2F12hykzzs9TBhj9XwILzjTGw1ydBCDHqhtsMLvDzMcGOqUBjVcMtlaFlYZcyzMdk%2FHE8Hl7tpe5EIIDzeq0tPzEX2qvbRyU5lpk0hyliY%2B75vVeGno9K64fEeanibkqNjo4pmRHE8OO%2FER5E12T8VNro7icpnljTGmC9y2fvdsmrhvpD8M8O%2B%2BtAK03ToDLKVpfLL4lAh%2BjO0TAYFOAjuZTQq%2FPd%2Bu3FL8H0J6%2BU2eknVKkmk82TfO9JWdvrfIjRxbSfWOguYBq&X-Amz-Signature=a28dd205fbca9d245405f4c1adf168fc889699ca85312f29dfd4d280302c5563&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

