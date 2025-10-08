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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46634FCGF3K%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T160040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaCXVzLXdlc3QtMiJGMEQCIG0qa%2Fj8XurDEA6APBBck8PqgkRwF0rf4w%2F%2BJSKYXompAiAyOlNYcVjhf9RxB20yN78ZhYwB2rbVbcStmx7jAMp4uyqIBAjA%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMjwPFYSfr0i0VmC6SKtwDxvr94FfilmJEwXqpIq%2BvOJFtVJH4qmljo5h0vtWKh0VaFtyZ1ZGx5Rl4zVv0X9r7NLT8e1brnqcTstuXMmQnpDUdVCV6zqRxLfzXwX6O2zLeU3%2BWl8HdCHmuLOpSVl3lL%2F03pEinaefl2b7qS4jOQFn1N5RqjdHVVxCNvw8OpmEhaPTBvvemprjA0PpfMqs%2FcDPTSwwpe8I4HhsxXJLeu94rtVWvHPtTH2Rh%2BoRCTCoJYp%2FBFLbT1DDabSCIfqpphhVIEeZ5kZ%2FLsDxT8UZrDFE%2BHWegyQjdtJKlih0mgb76ENqDns9EGF9up68jpkP%2B5yoYkPU%2BMZMRTcFlKnGZS67tEuRbOvj2JIp2g%2BfGJ9EWp0L7iNM5%2FlnRxnhH5mTJe5fgCuvnOZwVSu7HQpNo6JbKD34iWflCTFgGHNW6PiP5VCkUFGeI8rKT2y4yb%2Bx2yKoCfkWzUJp0x6QEL%2BTUfd5%2BAvrzAMPSu80DcKp6mZCuvBP19SWsHq8qexUPWM1I%2B5hTAqNn2y0aMqsnrSw5h%2FF0Wcq4yXJMioM%2BZ0sZfe%2Bo4Z7wjvixE8nYoiSb3WBYZ6YCCfPg36pRken0Lvyki2s48NDSFDOvaTD2rDaXZbMMcOXrXBudJ8tjXqEw%2BP2ZxwY6pgFUUxTpok%2BUj85%2BpD83aCf3DRMrJjLWB8JDbKphahWlGpX%2FbPqhjRX6DgWdBewQ%2BF5dm3KZ7kPwi4YEgRUquHscv9oN9%2Bnmr3oawcSsom2D7XT%2BZOnOgU6mGdvgBnz6xqS2i7LTTAMl6dOfvN0fMk3ZBpnrehb%2FlalDEyPa4zEMoTBmz468sNmQT%2B2YhUlA7bqo7D9NZGV2kAZGZfSNcRxHlUZRG%2Bz6&X-Amz-Signature=b9b67114b7b568d813f09657d485484984a85acd384ace1c3d4a1394e2fb24bc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

