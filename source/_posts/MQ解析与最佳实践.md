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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664N5SYXG5%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T030054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJGMEQCICZkHplRQ%2FnWZNYJK8pcldwz08fmAuW2goPTP%2Ffy8AgSAiBTKfS1An5IUuZB28sLW5BCeWXgxQcvLpaiLFVFRlwCiyr%2FAwgjEAAaDDYzNzQyMzE4MzgwNSIMtG1jxBe4bsgdXp9pKtwD9Gee1%2BbrHGquxEy9mTpQgIBytyz14nOc6Rcnzntj8T7qKOPL3Iqlo2kDmB4rLvUG%2BaFuYm%2BzANvhpC1Pz74oZzyOM0y%2Bj%2BqaM%2F98H4vBr0SaMmoZauJukbQEUiStY25hqHP4syGfgBm2woduCCMIY5v6%2BGK2ubqsodrSymC2RZ4bGk0GNXdtlMwDDUAYiUzTfRCoC%2F1r%2FHnugg9c6uuPVCVqKhj6p6ycJ6iHHss%2FvO1ARo8OYFw77Oy3HcZ%2FVEV5JocYxl1QP3PegJMhqtnbnymoXsc442pDtQNC6Q4i4Oh54%2Fnv0wOKCwiKaAYMmXtoXAnc8MijMT6CrZss6Pqt4j47ydkxfNFkJOuXX7nuRzyc%2FQlJYDP1oE2eVppXMI0gn3SX6M0lHPEawRrZCq2GH9QDKmFPwJDVspxXtUZOGLKzFsO2XUwWYIhx3kf%2Fs5%2BnouNIp51VE7vwdO%2FVvDXMFfYbNbdS6KMyVm9do%2FN%2Bhj27OA756hBi86AndzKepb2pLAjRmw7YinX7V5pgoYRR8jqmOdypoeufAGsJjvG4w%2BiWXKaqU2f3wOtgqF%2F8O5pSOKxQ2%2BC%2BGozD87qSpXhx2eXST%2Ba%2B4EYzF%2BD3tbEfLlUqkckNOlQvtuD767YwzujgxwY6pgFGLOpaUVXnOSjHmgXD2on5vKx9RP4JSVxa406Fux2aE3RlOl%2FGptm7xecg1QA%2FlYYEbw2QE4zV4KCqGngpuic3VDtvEK8KrO%2BmIqNlJt6zVBPEnNopjFv%2Bu2SuPIKZVa%2BI%2BonGdLfcN9Mrorhil%2B4D2IQ1HRZhmje%2FXJ3TDxiP%2B%2Bu1GB6nht3B%2F9njF97E%2Bugv6lToWvW1kT6Dub2ZSkSIkTPuUuTv&X-Amz-Signature=0559656a4e4a93e90f217331306544c3c726fd9ac479f3728caac7624516cf94&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

