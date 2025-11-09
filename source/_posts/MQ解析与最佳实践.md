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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466REKTUSYA%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T060042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJIMEYCIQCfSw%2FHwShz8U%2BQLgPPNKzNAdlnBn6AEzl2gI7kEqT0fQIhAIs3jBu%2F0k%2FbxPCaZri0o2KkLKDFScedEbjYZeIY6OvHKogECOP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwVkJvrXxNVUoR4okUq3AO0KMTXLFisZIVTFMx1xOh73%2B2QwjZssR%2F%2FtuNgstvDU%2B8wd%2Bbir%2BHQLBNP9q%2B9oYvw8dqQwog0fpRHvkZtmAtIt3fejJmN7hTa0nOdXqCi3k%2Bv0xxY%2Bt3nfHwiD4g5gmP32KB5BdQQ5ZYovZ%2Fi1xwANeIiRWgESNbkEa1QKJEuhew53%2BfAKRVg%2FLx7g2AZ0HtROQtXsTPrrukZsi4tbufgrZXCXN19e7bsorqf%2FF3Ac0ryMuxbV5a6fd1t3isTWQwJsBFMSLe6vEvopQ46jJ%2FzTgiL0Xm2s9QqtOG2Mvk7f1Kq8JGbcbdSqTTlnv9gQrczNsTdU4U5xx3lgF%2Bt05evBFHO%2Fkjm8QcqiVBxTjWPAvCetiONgFZF0g5oqs2Rz22a56SupnVziyCf1fptd25Jol%2FPaz93Mq4m6z7PxDAPyA4OuZ82%2FKhLJ%2FrvNByRwT%2FdkzxOmpEI9okJMnNXAU1toU5MFlGNfpyo1Fdrl8dUxFcN%2FP8T%2Fud%2B8edqYl2w1BEzAnBrfVL9H3qU6JAXQw6ifxyDPrUHNY14R%2BDHjHyq3OpIJaUfQvNYOX8a2R2ffpkK90KwoU28jEJSSiIp0LwPayHc7wNdSKQ2Af3GVPD9PJOy70bt9ABETIBy6zDX7r%2FIBjqkAQTt3Nlfu2XvLMhFdvmeFpDvEQEDZoMeveFgWfM0CWA2aYAaDDWJsTgwe7fSMsvcWjGyMMDMSVeYURKQV6Wzz%2BRiRPQrSIh7UilKxYSCzgRHWU71MpOW3iCUym8Hx4HldlDh12igWCFkkiFzSVIKMKKxfXTeeXDg0KrOvocWXBAxOcPUmBLJriEL94GYRexmnfMmehy27U30E2WTSFGo86SA4vjV&X-Amz-Signature=69d2a5a14f45dea6bdaab6af3db8483049da01e7ca8774b9a49b4425f878a22e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

