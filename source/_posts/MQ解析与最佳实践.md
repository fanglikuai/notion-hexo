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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VVI63XWM%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T040045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEyvupk1v8RVL7nteY1l2xe7%2BEY%2B%2BHG%2FpMY4FIHoUOryAiBpfCY%2BfihUhyZMm8ayctmX4SnlSla6bsQqc9WkD%2FDRUyqIBAid%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMaxQRgVSAbeUWpNjyKtwD%2FM%2B6AUJv17ieGCf8Ldvuj2fG%2BijoEQs2ZNg2HQMFglhyKwODmPSVUoWJQ%2FasbAjTYr1V3S7t90By%2FPab6OyYaD1gwhKTTH34Jq6ddc%2FLPB0kYSxNfN0qisI%2BvzZxOrI59y%2FRbFWW77KLOIdINeDV5InCRvjIpq%2Fx4k0k3%2BSmDPGwlHA79qC73wQ7oGbP62ja%2BV3duG3v09kzRt94mn%2BmJvkF37HqDbbuyyGZTIIJ5X2VXKUBPIzGuz%2BGp27op1xmoA6sZyV3BJM7hL48pwm9Hex3NIZuxwzq7BkoydwjImDAvaakr0Yhm9Bot8BdLw3dgTePejEKeo8J%2FBGCtt%2FMyXXDnTVxSScyPUxD67V8vs4HFbgGmJpHeXZy0c45Ifkl3jQcIs7%2FdHl27SzXbchS9HDMIZ%2FI%2FRmbF%2Bo04RZM%2FkkkvJLF1fNYhEhpTqQ8wduU1vcVF1GRL%2FMcYwenMiP%2FN28l8eE%2FuJWrpIgL7%2BAJ1lSqhFCuZO3w58mJG510Zx3MupPD1Rq%2B1OLTJZ2NBO3vHjLZE21frJR%2FxMvAkThbPxxJJnW2oxdgdV8jWO3xiGh%2BzJB2Uv93R56BTD0%2BpZ3SCR3p09YYKmCxnwS4vEr5eSEd5b3cBDveTZl356ww5837xwY6pgGx3gDCD%2F0%2F4T9uIT5KD%2FViP%2Bf1xklt76Sxcbi94IoODBrfo5pNPFTQPMo7pEGE3G3d5aBWzsttAYYY0qRGv9uoZftVQbsI71SfdrUSWCtqR6%2Fqrj8qqOlzCPLTS7oJeo%2Bh4%2Ft1Nyomm3gTMEJUtqVqNkODWBliNE7dZ73%2BfOHsrWY67ILDLgLJW9rC4u0%2BALKuhpiL79mAGKGt99F0wM8FHVT%2BRzxC&X-Amz-Signature=bb12f4d213866134c3f27b54f6f0ff38dabd5fe3b18d6cee35e5f07fa250580a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

