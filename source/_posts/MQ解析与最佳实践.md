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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RBA2HMEM%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T190038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDkaCXVzLXdlc3QtMiJIMEYCIQDRoHUgzXLWo8KGarvtmYoRvK3YDDLVjQEiYci%2FVdF%2BbgIhANlzljKLJmqaswTsAHTs0oMp5cKRCHEKll7kvzWWxt9yKogECML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxWEDMAgFmfdmC6uuMq3ANYqW9xmg2ZhHfuHSBfarEJf7Lb5HNEnH%2F4syjDwvIrcgu7mCt8Fuq1PFHSxsvF1wJpeQMAQMRgqPcvFYOx4la02CR5sZVSz%2FYm3h7G%2BCQ%2FjT2JI7QpmE8nEdL%2BsXOQDFKJNgbIQas7ZGUQ2zNn%2F3uMKPqM%2BOY%2F8GoJwig9Q55rNVMRTkXrgajTffPESEENuLYHNyF4M%2FOBnRaDjpAEoDMEz5%2FRIFtvim08B8mfKPbYfdkU84CjmytowlIkf71%2BL8WY0i2S1Z4pIuCNYMsGNfdzf4nDQiDXTTTc%2FJikLXCLwwYM45vlja3%2B0%2BdU5BPTcyZb7OyhNKk2juml5%2FkxceUAkAgP8PmQkmx5R3YdRHjzzaG4A0fZw%2BcXLyWOmolHgapeJRdjrWqpGyRizZRBTg4q2ukBgFhh9WCRwMX7i0I3y7Tt77%2FIAy%2BZQ7ebHXDkY45WonehiMSGCS9wf%2B0LHRFlSMg%2FOHgG8pOr1uQYfpr4umGZ5pka1W6Yt3%2BvPCOqfCwkerbHOfruMlgvTePz8us%2FzNYTzdsjQjf09yOt1eB%2BlYocFmAszIMeULixTQdIHtkTQnJ6udKK0%2BezrzS9jDRyqpPIUY76M1m3JwZPvZ%2BARofAD7Ejdccnm35hwzCq1%2BXGBjqkAY%2BwHjt%2BLvu2IDC9xiiuolsHYz9G9hZJRiVLVhWgoikuWcbA1Oj6hlVlddfESPLwJbj0m9doPlr1KKmvsdCEInjgj3QPN2yIZJG9gXLoNb0JDQ%2F%2FsIX%2BnlxMGDzavFSHdIloY%2FgOTtm7WBL0BFcobMLlQWWWsgiVfQfw0jjqz%2FtNaHSoWP01wJ3quLK8XLt3Gh18ZT5YEfivA7sYZXrghF3FM6Up&X-Amz-Signature=f0ce70012af42c232d2d022d92be1b6c6472846e01a37d3faf141c7b67593d7b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

