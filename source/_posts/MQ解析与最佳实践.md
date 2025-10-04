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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WNZWFQTG%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T070039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCmEaGOq2Adg0ZUjovQopSGlcUNmxQuM2nwP5MdYXoIfwIgA5xR6SbzO40bLRvqmsa0XQ8PaZmWrSeXJMtJzU3tKv8q%2FwMIWBAAGgw2Mzc0MjMxODM4MDUiDDTGVzAOL%2B913B9bOyrcAzvXMC9EF%2BwxaqCbJiWANyQ6CPqJ9LMc0x5n4zb9uEMSnC3ip21gnwkkRjG96SEY%2BfLTqu3cWk1Kp4080Vpxnwr2Wn9Nc%2FcIbwTe2e37s6lLTRvg7IjsSXTTzrTVyZOGwmgUDU8NDvzPqvYZ7mcAB996AStqDknkp11q0UE%2FG%2FukJxXEsCaDtqdyxI16GkSPW9TsAMWWH4bcqAEbBf0s%2FmfGueBOkQKiKYAJ6rQrsld5ey8baKhx3x9Zzi%2BivX%2FE8nebLhYgk7yYiCnJXGdO6zu%2BEMf%2FnJaJ%2FQREXPOYCoLq70UO2eeZT5J0LF%2BEuXZpu64zOo36oWUZZmbBULNi1hNmPZSTOVLlSXHUfDxfIOKe7NIVknzdn574fJOj4CGWewvrBrIdE%2FgEKZbwgXVFEbyx3KDT2HwdL5S0pLLNAsWUIbapKmUS1IHtnkvn6jybFZwHOYgRhEkbYaATIcC0zEYUDEq99uf1et1JsZzaY7%2FVaZLUx5nqdNrStfcywrsG2%2FKh%2F3Fu8xYUKt81DWQn2auBsAE%2FZWHuE5pRlVYmaLX3bLkPJSzjnc2DVLUU8QSe%2B55sMc5y%2F8TW4f5Za3K8HG%2BBZQ8CxvxcR0s13IFjILok78OT0%2F6SiSSdNZLYMIf8gscGOqUBoTHSZkyPfvVAVWTJbzWnQcf%2BgzF9EZFuJS0UH%2FnnDVtw3czTx%2Bu%2B%2B%2FF22wuMWAc99DQIld68pUi46TeRrt8MURUlP8q4JnfCd1fSRtKzcjnaojM4e5mPZ9PBvVebL2ifKL8CzkOy3CtyQpDxaDKySjLcn6yZATAI98FoEhDCWcApgf%2FHGQaM9QAozjaxrn0sWkeWm4LfinM5NSgIecsDxAzRuKh%2F&X-Amz-Signature=e98e45f246305d13ce59dfa7e273cae8f2b3e96e7b15f847606909c274abb8d3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

