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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WJMQ2IOH%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T200038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD9zVn%2BEGTDkRin2GaUTOn104UxYsWX4I709Kl3dA4FHwIhALrazpRqspWOxQwzDvFfcoxQ4Ug%2BuKG5hgYMVDkfWc%2BiKv8DCHsQABoMNjM3NDIzMTgzODA1IgwZqVlJLCPbiFiVy6Aq3APvoXOb7GIw5OEllY71sokq7apv%2BtOyNzx6w9YJUBnC7CjI%2BdMk4QXcjuJbQHzbDOoyN3l18lIAgG%2BZqcciLejIy22o1bqe%2BCN%2BLc7zZ41GFPQbMSDwDaVKKZU%2FJufpLCgnAyCwi9BTGAVhnC607rmzMjggKp4Iz%2Fci%2FnBUfdL8SwTy%2FxiauYRRjz6T1oaGkgqhBh9hnbMFCccUSfWJoEeL9F9YPljS3wcAWOo2mc0qLRnHRxArVmyIf5HqCZRua7pcSM9TLwrDIjh%2BMkto3Uylc1QcR44oAz8DNOvXi0DwlY6%2BeI2A%2FymNkKfU5Q8QIrm8YZ85z5jGF7Q7d%2BT%2BMJYCM6v8SLu9ShKCTEeAuCmLUSSh7STIGRwe0WJsVWQJJUT9JOlN%2FqGoY3%2FIRm51r96%2FpA5K5euSOQFrohe5rEyawBdb57dig%2Bknj5yrNOXPHhCLZJXAvpb4bq1ZckHOYR7hP6OvBzM78%2FF37z8984zzOvNVh9Cj4ugPCgJgjI2nMfILcOssNM1BQmWmJfhmNu7r%2BxKK%2Bot%2BLlsBtmVAOewsApvSuG%2F67D1fFzasTyubv8d6CUN0Ewgnc7ejzRs1l7LvN5yd8O3O9A%2BWSjz3P51xvR7S6rkAjYWGc%2F1zEjCM6IrHBjqkAfO0VCeGUSbQ9fSJZKZlTMJ6WH3BILAp5fCfN6axASCUg2ofPLrxKBEDwkYqnbIsiDUII8R4MFmpEO5KFk4%2B%2FiVhEtlhAJdheZo1zv10SWNrZ77Bwb3frmFCLo6IMZsbFcFZIJU%2BzQv4x3Jb6e32Q1VxFIXKvfL1eTlQVhilVd4cGJIDWSGSSm8UxTh0lkrkzmmkltw5uD%2FnweaO5T3d9A0IGVzB&X-Amz-Signature=a56e1d2d2f81c77ab23e85dcc1fffd0183673a71c2f32d7166701f1775e4f750&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

