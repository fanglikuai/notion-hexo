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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663FDT2UPL%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T230039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDcaCXVzLXdlc3QtMiJHMEUCIQDnAhyIks9GaygHFzoUOdMStwq3xDLC%2BPm6jCtOdRGR5QIgEXpoSrg211cf07oIG0YBOR6wnis9Fe%2BwuFgKlFvNk5gqiAQIsP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEx7pgX3O%2BuLQh9GBircA2%2BjSLq9F%2B%2BK%2F%2FEF%2B9pRQy6NV3ppSrGvE9vTZgCl0gizjqatpUkqmf%2FF21CejqtzVhhciZdAWylXW8ymk6kVV9gLOQJD21OR0mU4Fhg4bAtb6wrvxTXXD1AmN%2BSIL%2BZ%2Fqh1CdsaZXT%2BnIJg%2FnQm63KywXDjCkUzCtrylRpw1SiZtMKJvWT0jBPTrdweEqsfuISJDA3fv85AZbkpfT%2BfK1FkAj0oJg1edUn0tkAwv6Znv%2FXAj8CE09JQXEosqcHsN7I%2FNA9d1BJVKXbkir7wmtcaZSk%2FJ8XfWc66BabUagGKDqNyYuk6abNnV98Ixneik0nRY%2FydrQgZk2L2TtUpBRw1Ns%2FHW%2FQ%2Bv5FO6nS0bj716lMpdV0mjbNBTI7j5Rkit6kPjcFo77vUlBUL%2BMhoNbljEvW6kb6MSUOcMPwowNucqW3H6pPHiZ%2BJlg19vYOy4T5Y16YySkB4xLuIbITLIjIeY7OkJfkDqYXmSJimZVimBfh1MWIblp5o5STvQnv0k%2FmifNfE%2BPubZYSkaPUwOLn7dn0z%2FCxauZZcoY3g0sIvxfDSTRZ4uhmS5smRqpxYelJTOLcXS7xpJzgayHmJBhOMtiYDFUgYau4UJSNZ793yH7oGjs7zJDMRZTEhoMPz1rMYGOqUBFsVyuhM1xlgJaBnbAmapjJebr%2FNOTEN3GgSFe%2FzK149YVh2PGPZPFf1CtQCFXbMqo8oS8a0Cufe%2BvGEw00nitsidC1UQRZTn4QXa0N7qz4d9nADmTeeXAenvUnTGeZAsSJSYKhKwL1J5S9mucf%2Fo7YKLyap0VmjXVSKuEBrPQrz4i%2F%2F1xpJVPx5FyTJ3%2BDS%2BjokpB%2B3qcKUvSi%2B7SokGwegxEv%2Fi&X-Amz-Signature=a114285325050ded3c5cbfa8a4c65f5607f6aee32ae76593d89f5ae35f420ed3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

