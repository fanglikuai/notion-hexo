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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VMMED5KY%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T180050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGkaCXVzLXdlc3QtMiJHMEUCIEwAn%2FMvuSNQzfovrFB%2BzzzWhyNPMsq1fsLM6YiCdY5%2BAiEAqhQfzKGFcRdC1CQ2h3M6h1QmWSHBPlpdlqFf1Z1TbtwqiAQI8v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBQ7LSuKREgcaDPpMSrcA0Xffcppaat9xQryPiccR4RlspV2n7UDGcRbqjS1pC%2BXAFC8BWjLU7TVtKVPsyAw7VsEkb3AKxzUdZrEKuVNd6VKQL%2B80oRnjrBQ4vyLbjOCQ1RHnIArysJkI%2BSG9S3kL8oOD9aScTdZENM6kDvHfSKRZ87VYPvYngEf%2FBUinkOLpiwmeKhPB5EslnrkZudK%2BP5Q2wzEpS8FTtUw4EC7OcKACpfgFUS4ji2obsTy8P9jLo1EUCUr0apifg2N2hKEZKFz2t4ScpMF8zpaMtNs8Ra%2Bqk8EIKUA1LDmndgbg%2BERDhlFWrHTNvZiW915qDk0kbVq2twvaRWHmfR0D67ikx2GcWOWeRQYR0xuuVa%2Bx1puD%2FqlY5csaoooqQWK58o7d1I7jjw8mQhMNormaGBizbpeFRvSPJPRJD8ZRMhipnp%2BCw5LQDRot6l080Yf1xlJcEx4ZE%2F4YsRQcGyRdi2TY0Fqq6obj%2BghplKjuLMpf29EtIorBdnN93VbQctjAsEI4529up%2Fuq9JC0SmKUVM0YAzWE%2Fg8mdhMqe5IC1oKeyCQibj71TzqEECAfbJSG3KFj%2F4EWTt0rE7eK1Kv1okwexQ%2FWAgP%2BaKswxe%2FVEE3N4NTLvtOqOPAYY44lyKZMKKX8MYGOqUBiXhIVoNTstAZUGmD37V64lVasAUOdQ2rzPLhkw3039Zw6L9shicgjBwLbqleAKqtZBYBLWzXNQXk2n8ad6FVgEjiC9kvM2MYzO83t%2BtBC8BI%2FrngSEdHfbsTDRkMPJDAZ1RROkcCe8PTd%2FKR9gTofJ9kR2bqWrNkvk4neavp8tffzjh0ZlIUK2Rs15qoT1uS0c36lHiVzEAa0nWYmPi95W9C0hKB&X-Amz-Signature=a3d87d234c748908ce0e3bbc12e1514274bbe5613e93869861838e010e6b15da&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

