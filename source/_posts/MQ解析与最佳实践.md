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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667GZATYLI%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T040054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDtos9SYJP4ZxUGU%2FcDzh6lVIyHvUepfzbHdrXXiaQAiAiAlPyE%2Bs5C7cUpGcrrSsVkhzTS2mqHIra1KZwfKfY1mgSr%2FAwh9EAAaDDYzNzQyMzE4MzgwNSIM1IDm6n0DoxsfMLawKtwDyHm%2Bb0zbo8K8SWyoYTpIS24E1M65FypwvlIeJOFEyk%2FZtspTFIT0R3sQ4KXPc1WQxoODHreiXD8ciCrBhbFZN4qojT2MQTYL8bIf5BSDt9NLymbt8B2qLoXr4z7AKs9eS4sYXuqfhaj6daYOd8i%2Fh50dzM7UdhqWwaJAZyuDX0Q3yUJ5pKJ5plTpFufvS1ilVkOjTmcqhhTrrDVUQMKcqUiTZYyCqjL26VCALvuHddg4s8HyXigLAqkQRezlRkRds%2BpEjza3nmONwW79Iz0NsoaqBff3NH2mGo%2FufGUCE%2BLHgUQf06i1QN8sdxa1%2BCmmDz2IeZzkyEInPZr9pnWIaRjjprAwXq8%2BKsP0Mrgg4XkRYmypirlikOxY45G%2FIRBhGdTQJ4aNZ3%2FMQPDYtKEiAr1YQvrSpK2oVI5SN1BwzEaHIupdd4T7MnuJhOWyShVOKxt3xbvCHh2VdZ3OTEoY6g6XEs5YakTr1P0Ir5KRrOkMXKb%2FIHjwn7iDtQGWejcuVbMnKQuLFnpib8px2OwK%2F7UUMImEvvd1B5%2Fh0H1ejNPUfDngQdRkiXiVliqRZ9KzPlxBB3aPWWU9sDll6fcTsZ4YZWMtxIuTSoduR62YwchTaGwEKtkrIUJeSVIw1uqZyQY6pgEn8nsu8RCb6hh8DnGdJby71DmSXnF5Wz9QJMYOWdhvmoT%2Fmm0bg4ftkA%2BRaQyBigUrnSsJye84MJyk1xydv2M0p2%2B1%2FZMq4YT%2BxbVPSO0R9hXhCwBsgiykZ4o9Us%2BSBj%2FUwZCayuj8%2F1bnHcO8BuMhVcTalgKI1iaxs7YYArRASfLyNw9IkrUsvkVWWif42FOVHMgXY%2FbpnCXje1OIdSlmSFUwG1Ub&X-Amz-Signature=f40619ae7417971aa1d198ef7907ad4912f5f1cc83e5b9203ad3b3c473a185b9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

