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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YLBPEIEE%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T120043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFwaCXVzLXdlc3QtMiJHMEUCIQCaYY9dy4ti9WPXGqV6n1sNM2QOfrTJgip6glo2o0Ag5AIgGkPRbLxyo4Kr7wXIOP2mFttY9ANlbknzIFCyH%2BVTZe0q%2FwMIFBAAGgw2Mzc0MjMxODM4MDUiDKNwsPhrNPHiL7GcVSrcA5UTJVw7lkJTdXfoChhApMJA4lP73dDPK5td1qn%2FokFhQyYca3Sb5Z755BxdM5ONXRzaIp%2B7qC4%2BZqvBwoP8qlDk52HfB%2FZ%2B9AFn77Y8R%2FI4D3bVUmJj%2BrgRfjx7oNOVBcJoOnusOUhrG3I6nxefZT0AJ70I1EzgLKHAPyziRSlSLdIEyrfGD3j%2Fc6fA8PnyPxqJQCode%2FgrVpfYy9Yde%2F5vQ7TUanzOTXC%2BzHVIX3YnGR9nvCBSlekHv0ISFuPzkseaDc4Oz8P9qPEYX2SmHraVC5KfoJAzunPucgkDgylA%2Fe%2FWno2WhrAes0pBq5Tt%2F20pZQbRY6JFYguXl6J1brgN%2BYy%2FprUrERlutqPCbsOIr2XZKtWa8EoiZ18NL9ftmy2ieiAyz5hHl1Bmpwri7J6u2%2Bsca3b5tfvO8avUs%2BwKFT1j%2BZIhjhW5hqXbArBD1kmEL%2FGv0OnzzKWlR9NxBToupZv3Ks8VGKWbhBSen40OmitkzBhhADrOQJiHxQGXwjUBEmON8oXXWeYnZH5PLD2Q8sPcyceaZWHp59DQWB5Qy%2FnAtXSTolTiRqhyKB2SJjq93gYch98UX4Q5kPrjm1K2QxS7X6od%2BWvJKzMROsZeqi%2BkA3I5yqvzOCdTMK3Z3ccGOqUBQm7OxcAZh1J8opv8BE6Zm0ged%2BJic9md8hphV0%2BZPKNdbaIGKbOZwodArify45guS5o7pCO0GPmneLwciwnagyRxeLO7M9z6VAnLkpjcbJmkqEiRazUk0F7hUwXxTgikoZjLLw3G0tJK0vRGaacYjkcSW1Z8L%2BfiAx%2F5f6%2B6fJK0ekiA3LZSgY%2Fgn60KlwipGAo92qKG%2B7iH%2FKt53DWR8QZTjaQ1&X-Amz-Signature=a01f34d87abf3541b10ee2e777d9c705630d140408127e1eaedddb4f35f47638&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

