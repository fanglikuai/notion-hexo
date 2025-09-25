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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666GUDNUOG%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T100043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCjQiEnXiT9qxqmO3yxK0c%2FSZraOEZrfmsm%2FheGgKxbTwIhAMhsOHU8EQBQnyY8stO9UGo7evf70Bm5zaAcQ6L6kuOaKv8DCHMQABoMNjM3NDIzMTgzODA1IgwcWHBL2E0aqqiSFBUq3ANk9a%2FXE6w0U9ZjwqBPWdqvdnk7X4tV5ACuHEPeHs5nWKaTSRsQBTnUAUHxKJr34nC6w9mnk21iFGnnG4M6MkEsW5SKaEtXqcR2c0FzIg25GMP8K5zYIEmpepLZKhVRjFV0tuUrR3elvC558ul53Py5n7fUrRfrDMYKwgA9TRag4ZqDwqrLXbyilR%2FTm42kklQhrZxJPYjK6H3r8XptpbVNLrSbbXYJH9CNY0d9fLLRnNhtPw5GSKfh8z6zG%2B53ZjqjginuH8Jbr4N9nLVXTWH3XhSuWrw9PMs%2BCHvE4a3AMmC0xQ2RlFktqJcUnH4Z7wUvFpL6k1dqphTHgXkncckgEL6R%2B%2BKqYYIBDUGZRMIF2HiLwQFB6%2BqVfHkJtdF1aVmRLUEekf8ib10jqk2AsldsOW93TeLA%2ByaY17PiM%2BUTnH5OhnRXUDqrCXGQMzaV7m04TJOl0w6ZRc2Q4VG3BjN4Z5Qj02LXKR6xz6oDLSENmm7u%2Bjjcutf9NieEG1EiXMgzlQIBmjSI84FdNqrekrHsMqHiQmHQNbHYaRj1WpMe2krLKu49CwU1B3DXp%2By5nUe8nq2cMhXoi1Vr9pf%2Fhtl0UC4EZdK3bKyV4wzpHkT%2BjZu%2BEw6pA7fDLNyZvDCZoNTGBjqkAU0GKDFpf1Fhp7M4DYTeCKuI%2BT7g%2F36CSqeDHqBJPgMG10cQ5enKelcKuYqoQ%2Fi4iaiFbjADSr962Txh6M0IDv1ZH7pXXuWHOCpb8dhPmYbkq4o%2BPXpd7UWcR1a5pkbRMMq%2FDCGu%2BtJ6utZtmNajpoOt9Mg%2FNjoTadrrFWgh3j5s76XAvFMRDtrFwZ4JGUk2Hu2xd8Kb6bX5RafI7aKTQDuAZIPO&X-Amz-Signature=bdab2fcab2a56847a979cfdc7cc545f43a6c3065f29f0f58772e01b8ca81109d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

