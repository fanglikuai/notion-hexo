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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y7YEIDIQ%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T070038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGYaCXVzLXdlc3QtMiJGMEQCIE99EJtcXpM3shgJeHjnNEGCPPp5B%2BFEjZFR8DmlxylgAiA%2Byw5L4q0yRaqv%2FGY0Y%2BiQXvqu3pH2ov2Aj1HptXoUXyqIBAj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM0CuOAyAS6QjQp%2BoWKtwDLQ1%2F6Mkg9bpz4sT%2B3h6h8Q58T%2BSxJn72LN9KGxhGu0r7m7YeVTmhL09F7NTCo9xtuLKnVvYwv4O3xZw%2FQOh4ACnXT79vgMJcwQ%2FxSOY89GkehK7JOPp5XIKdgqqWoDIFGhmNXq%2FC%2BmrnEFQm9KC2UUWf7%2FcCk%2FbW6p3ZudzjJgYdoxfmbJnv7CECUFHpAICyqNlbB2Za%2FjGJ3hdUgo%2FaD9%2FmVKD28aYMPTNPbD2j%2BtRA%2Bjcfn7cLOL%2BB%2BJt8Gy5F%2B97x7Mmzl69cEk4rqWmDO4wCHFdhSNdIDsPu9MNj4Rkdk1KWPAnbLOvfwRORjdFE0Ni8Cy1opS32xwqvfA0naRCYabCwNt36zlxLtwOxbwiohLy8kpiHsU2Hx0LCrRFEZhbtEeMRe58iWlzY0n3yrwyX9j6Iuy%2F2ZszT3rhhAw92Rz32v9X3Do4BL4yQL%2FrQJDomUX7lc0K39xfaKcxfzMdnWsZWOK%2FBPu9MUR35dCBrU00Ia5RwdXKm4BRLwMqI0DfsLhNmT%2B7lzTj2cTBNL58UKN5EBmptbFE9Q8lORH5JNlI09lNWxgKhuNeTt2sNu7UW7wRg3oNVoSczlb%2FNc2exjwpLt44YiCQ%2Bs5TK%2Bi%2BpfcTtFQOt%2BnY6GSEwz%2BKnxwY6pgEY4LVB1mCa5TNW5blaD6EX9%2FgbGmP7piPEknJwUjvwu4pjMwbTGBg6fo%2BIN8KaAAaJCGaxyHxwAJyEXcKruRoi4YyB6j%2BqpsPe1iI8RGw4KVPjYQeJfJiMQDaMSwaYfPwcJK5UKo4zXvRieAiahYY6QtyxsDmSpTh0kHcc0Ng7R1TJ2FrY0O7bmkkgAtsUwqYi0u33Rw8hYJKSWBiTHStbG%2B%2FOaxAs&X-Amz-Signature=3e9d408563ee268e131eee028959c78ef80ce1c970162f598b9647c0ba8a5729&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

