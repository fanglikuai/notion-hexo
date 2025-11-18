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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YLE2HJ22%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T070057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIH%2FtVPW5OzXspprQBLZqnXZCZm0kiGDQ2gvhARZyZ8RRAiA8Nno%2BSUs7GEsiO%2FRRUnjJZQzpzLIDvazqao27QJ4F4CqIBAjA%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM1CCF6wfgmRQPN9lJKtwDbgg2hhMRv0YBGoKLlDDtZJgtgxyeO2uwPCmSx9Ius1Js2uAP2Lw8ttZkewIKCn3eo5MMgoBOEueVGZm1xHqMcigcYJzuHUjgfFdVCns%2Bs%2BoQAua6V1wnMOJwiSLt3hZMtAGmfRXhixktnYkG4FOQAVB4rLgaoQT3DxpLFSU2Y2C8FaVEetEsArYapLQyG4fbLPqogi13AgTWW9sH%2B1xWzB8gzkENahf%2F8JZmooM61PQIjxlvQZ88LnwJlGGoVYsx%2F6ABv5v72vHswGKxOlTYZtRiTme27VvTzCMEMrwxXLxhg9NgV0j02z2djiLD7nnFwEouLlMyTQhGEoTf1V6oGOXWlru%2Bde%2B9wH6RGbxiF88n06k1FvzEaSDSOIXYblrzmmCD2gawCI3oRHWOcC612zR%2FT4IsADVfIUhB%2F5QTZ4fj2P7ncb9bSZB0c8FWrrwN%2BCyFqAljTwjdbL69gFTpunzgaz4tXdeegO0f97tW58NsIhYtsXona30Y%2BAmf8BYsnXK2ebP%2FnP4haE3vxa2sSFiQ0O0mvaj54YsL%2BPXnLU4rifitGHnDn4b2rbgse9ljQXgGu%2FCk9sYvXpWT%2BcDWLtKz3UlJEn3cZcvO%2FfnNckkP8DUbKLD%2FACNyRxww8aHwyAY6pgHqh2qK99L9cpEZvwzO2z7xuq%2FUAjQpPoOOs60SbpyzlAlcbimo73QAV81Z8kZ2oCK7wNVgPNgIOMv3BrDH0YxEZTcUbW3LUhPUqjQqqdsnv6SKjSix8y8hxxZYLWWOlUlI%2BfJPjl2chJ8pY%2BAo4CIx%2BjZeva88oycd7Q%2FiEhA23uKgh3Zdceae658Zzqb%2FvPyssjh9zVt54u5kY51OvOFeoO%2BMwbfC&X-Amz-Signature=53195a3e8c6ee026a9cb03a16a7211adf096493dc293f3c7ba1381db91842848&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

