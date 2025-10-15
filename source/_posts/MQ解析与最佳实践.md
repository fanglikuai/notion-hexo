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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZM3N5KH3%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T140106Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCGzfGGdDa5hCLC4RdRi5MXwDlnV32QNEDPVddrYDe4RAIgTg3%2FiQ1D%2BHvZa6kw4MTvvwq%2FTXqosoy8fW7De1kRwlUq%2FwMIdhAAGgw2Mzc0MjMxODM4MDUiDP036WER%2FXVV9qEEiyrcAw%2BYkzaaMkPTXfuF%2FftogJlu4uetnk%2FVYpWmILr5r79qJg0J5dUlL4yIJdSeb%2FotZN6RcIof%2BFvJp%2B6elUKkaZ3FA%2BTDG%2BB6DnFWpV8bvuAmQQreYQTjxie1ZUvq7%2Bgk8%2BqTOgHI%2Frqa0nD%2B6omBmAGGIKobiow4jlih7sl9ZjjozTEjO1FWO5uVAcwkVr0p4MvTfa2IpJEZ6gq0SeHDNwGbZW2G8IsWIV1l%2FoqYeJTz79P5QKguRIHHAkeyKXKG4t3E8%2FNaJF5jvSFfMOA95cCd2CrRObuFXG76bE0vHKcWuNtMJFYPyrXZ1n3o27bznNPc5tnA0u71dmA6FfDSDuxZ8OhQ%2FI%2FzYDv%2B%2FWFMN1At5JObwlNoa0GuFFLEmaXMgAqFk06FQSORbVqj%2BbD5LDW2J3EU1SEz5E6inKDPMKnfZH0IFTqpzPKH2lBAtIwRLIB8e5WKOxFyFDK7i22nmUCMev75NEegs6wWyW%2FaEeslDpAjqWadvapQPmULOgRKyR10o%2B6BKqYHz79AoHpT%2B7A8d5b4fPkqVk%2FpehQUB%2FPVpYu6JGKOhJ4RyDz3TR6woT2E5TNz5douxcZ6%2BxIsmlfsShfvRn3%2F8kb2Nqv8uGaMr%2BlXUrLbtbo%2F5gQeMLe7vscGOqUBrHvo1%2Bh64%2FxRVjtOa%2BrtYdiRIdB2SQ8c2sP6DSmyuCprr7sviISSGMRBq7dQ48AXGWBbrg6Io1pPzvEbgqiguuIZzd8enYHoO9oC4zxFW245R%2Bvxf8qNHBBeOB81xlpIuFgfp7XM%2FlEWRVFxPIgxBtnfj0PKKPwMipBU%2F4x2tvjdw05gDPPD5dqWAeu8%2Fi4S4wq2U%2B2wlYNug5rFzvItHN2JjDOQ&X-Amz-Signature=2df9d6e25131f76fd766410d77db1551a2116b498a611feac9cbf95d99563801&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

