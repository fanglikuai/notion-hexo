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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XMJ6A5T2%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T230049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGcaCXVzLXdlc3QtMiJIMEYCIQC8w6c1iGEyG7X21cF9YBgQqt32iclBUgpyCVmHFWdgkgIhAMaz%2FXG5e4sSORNRMXJxRe5zvP0FuVR2md%2F1yyqzJNbpKogECOD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igxaj1SScgkkj5qAMYYq3AN%2BniraGo%2F%2BlzQGaW9%2B%2BmT96U3yTJxhklGJ5X261QJUcEzdmbu1FDoewEL92CdJK4yYuP%2BgYBbwGu%2F1lE%2FXWh%2BPHoi1fwL4%2FJdXa7gUVUDMqcV6VZvW1W9N1XDAHPmawCpiXn4A9z09AsbwmuCqyRks175xwG48u11n%2FDfCGqhk2ToDO%2FXE6oIyLTG7RvOm8rGjhRhWenOj0vtK1nqg06BfTvoSLdWdQg7H51rFAzFyyOmunK6873PC0tI5U1JvSuNhpKfstv6ncNHCH5%2BWpsStAM9PWW1wK1c0oAo1pEsgYYEVOIEBqHk2eB3%2FjHEhPHmdXKf7b6mvS3HZZ36oekVabrwPmE8tyl1BPiKI7S7e72bcBKkM3W78%2FGv8EtHJtJL2jQ6fTJzHeNjZs4tuYl0PeyDW4H%2BK62uUvxUUYIm0RuZ8icbVa9EQjZc%2BpfIz8fz%2B6p9jDCILGVLwCMyRgP3oGSqaUmDpui06enIIvoYKbKNINVb%2FauIY8Z3SMbg7aEaxxyGMCIzlPgU1dK%2BDIbm4WzQyfSD1EihhoDhYbYYKAxXBVuyfcV05kP4uWoR%2BZwY7vN4czbWCeMq%2BPC9YGZcHAE8lysi05%2FNy1EmRbEJD51fJKYgWQIgIxfCxPDDNr7fGBjqkAXrf5DmHsk7XoI49WMb8WaXWqECpXxKzFQwZ6MK6xIJncfZcpStdPWIzKLElb9hWtuS75%2BgvNrLvK03RJoS9Gum8J9pGLzzvxdopJYrKb9DwJMntYRg%2BJOJq235hOqVTmTvGfW%2FVBYRV1BnAMk05IbSU2cs3r7h%2BG9KR47pXyfAq4lGnbleCUXSo2fkoZmtV%2B6hGjPl5%2FnKElL7cg5v%2BxT3KXus1&X-Amz-Signature=89d0184a657dbc11e70546f14cb341eab600ec4b163da611b8e626d17845c7c7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

