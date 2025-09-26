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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UQG67ABM%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T000042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBe9do6qoyKN7i7PervhuY47T3oMkNfFERBbUgM0euOtAiAT6PcOu1Kup%2BhYFIh3C%2Bv6xRCjoH%2FC05R71uya6taAbSqIBAiA%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMA3e0On%2BsRWQaTlrpKtwDic1qMa1ri8zUU2NbqiepKtKgyojFFDFxoH0DyH6Yb0JusokdH6%2BGy%2BSqkfxsMpLvCt5OVVB7luUFhHt6WAIHCwuD3a79OvYIpXJEqC69LgiSWLzi4Y0uCtlEwMEUYGWzonZ478qP6QEqyUtlVsL%2FMnLnCeFYepx1Ap%2FNnYNgzRe6%2B2breyyfqSTXOWv%2FPX%2FX6lFU9RJeuyYtMcpaQUqTGgjVqY3y27IjurHD6AviUwohE18O4xwxwtrEg%2BIvWEbOC0P2ANgqGJwS54nV4vNkee9WHcxi5%2BOOlaemTuiCvLBdoRytTDSNHzQl0omr0dWDt8bpX4F7gwrFDmgKZMFvNJYchB3Cw5rpaxDI7NBTvNQYI3ih92YsLLFAbImnaaVCRoqVI2hQix2xfzp%2BUzPZ8S40ugqeztfm6YffoPH%2F9LHW6T%2F%2F07Ef%2FHXJzrU%2B%2FbZOlxKYkcYNT3jKPhVJ3bNSrvtcY6ttJuDILtgh0mXH4FiMnf3C4qsonRc2WiJ8b6OpPxoXsAvQ1Dup8C%2FxKM5wgD6n%2Bws2zsy7XB0bSdHT3MaGkj0fjF1kLCx3RZE6wztO4JJL4kFaKW9B8euXixd%2FbC2%2FTobUwIxrwXjUYQ3N3miwVsHnrhdoueJ1KK8w4JTXxgY6pgGZq%2FAT8TGTbNI1bzFMuRw4wOQxpyBhO7Px%2BowX%2Fsyv6X8Tfd8Z3kZlxyd26vvELKhAiTUShyNOneno7cNv6MD7V1hJ3I1YlYwnozJoIpiPm4gZMnsZfpPMan8tH768onvPNWyLUr%2BMa9Vp5ip%2BX%2B0lCDcUrldIPAfIC2ebFGWzmi6fwekZ6VdHvDaY%2Bn1%2BYvZfUsJ4bcZyiYJLZ8rGe748cmxMOwCM&X-Amz-Signature=b895a3f88b00973765ccbcda47b29799f931c6d5248718eac359753970e4e294&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

