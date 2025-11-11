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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YI5DELET%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T010042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEkaCXVzLXdlc3QtMiJIMEYCIQC4Li5frRcxgz9BCEPo52qKc%2Bmy2AVzPV7iilFtjT9EPAIhAPvYV%2FYvElmUY7REBgK7W34ys6qnNQmHQEy04Pdb6r2xKv8DCBIQABoMNjM3NDIzMTgzODA1IgxHOe7qF%2BhtoazcEgoq3AO3xB%2BXfsT0GuYKP8SVvDzFKQ5iQ6WbhMDl96dgG5aRo8MtjBNvPltH36kKqRRjJcum6BOXKyamSlCD1UWZeLEo%2Bi9%2F6jmJyOL6jPBb5O2oadZFx8fyZpMyxAQUCKpK1Ab8fqfE1bcUYugptPdUq6HigRQN6860Rb0CsgiX3pNvscKO1UiTTwjGVUkaOoJJ7bgW7WaKQI2%2BrrVH4BdN2z6eDj6jfOQSD8v8weBScdTJS0RlBx3C6jIJO0tcQOyAVWE31G%2FarSmCzetmVWcqoAwEdrrnV813hODlWqdquy%2BUB%2BLFUVA%2BBap8RG3qUOzC1ZLxNOYvlAnVpk3Ku9Hhz%2BxHn3Wwhcweebs5VqjhVrKR49A8gozkvUTE9Q3K%2FEcEybGhGVouKpSOR314%2FY%2BJdB0rXp4U3q9aKw1sXnNz%2Fxo3pmF1o0qGiMYWdqSLlMZ%2BAhtMsVkvraN9i6BrvRrYi3a8gBCQVHEQc6CY2AvlSReHDKuow8mCFgAG%2FRZ%2FrLmOJK%2FxvvIbwox2DjS2inu0XUwiGRGfzg%2Bk0BKPsckuvQT%2FhapkScXQjl73g%2FxDprx5Uso%2BQk2%2F02Bf%2FSPg4zgVr4JKmwZPwO4bT%2Fm2HpKQDDgSqyxPCsoEH0FBAZv39zC2h8rIBjqkAdDUzgMXwpRaKkjdxj74CN4O8Bs6sDn2l5M1xNJ9L9DKSAggh%2Bniuk26VFzCC6pXn%2FKSgEnXzfArsvqu8fdQTygt7CPtSLKtv8WSZBO4E4H6AZSeK8k2RxKsk6uZ%2FajYYt40SREHJvMc1T4mOp9IRWwJU55WR5m6MEjtnt4teCb1LHlEhBmHuaqE%2Fw5ACYL04iient19DZ4c9YSTKX81WkryHMcJ&X-Amz-Signature=44557a331a7f4bc760ea808fe142f8a5964ed3e96408ff8c4a1fcfca4484aa2e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

