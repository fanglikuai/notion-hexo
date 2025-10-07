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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z7WVILCG%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T210044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBUaCXVzLXdlc3QtMiJGMEQCIQC3fGfHVeSZHSoxWAjuRSfgOtLgbRnJo2sVonsVheZMMgIfG%2F1%2FOkXkdDhPZhLgL%2B5jwuPEzIXpAwPWqeXSRIYwOiqIBAit%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMiheRmF0Topjacdd7KtwDxd7I4FB%2BtYM0lUbjGQQzxBdVChDc3SWGGpKpfcg9DpYbARojil6goEVjnC0ebOQsXx1UHrsRyNJ5k9YSaG%2Bvh%2BdmQ2%2FwTnc6gHz86wkEm3JrCF%2FLWgghVHTGc%2B0GayzS4RgWSz5BuDE1U1jEal1nqPAFN5%2FJ21A8TEQDKZbbYRQRg%2FkYJMQ1LFG5NYLvU8NaKFvHVZWmJWUdrD7syqTi%2Bv0v8VD8p64E81g0nXSP9Xj9kiGHNYujabaOeRsIOskEnRX0QvO8LHJESGDnF1Wc0Q%2FTjiuGSdCrqadS4LWlFHzTGRgV%2Fb3E2xCpelbk%2FY6erG1ld0y9NIZKGlfGbY7LEtyFoezknnm0E4wCHvmHewPS7yKsfOyTBqaIvSlq%2F%2BFxi5DYOT67HeTQORxUboCaKCzv3ypBSN3hMdWhkvOaDhtwZdT%2F2en2i7Bbp1tel07HLfoVndxh5WDofFBiwnyIU8lfqNGjPCvaco3vFn0YtoXC2UR3h3iq7hrX%2F4E5MeZRb5Nvj4GCvkD9zcgBzArBapYPQLapU80mInNPI3%2BnnP8Yb7eF2IQF66GJJqkZE6vvHKliWKmxnr4or%2Bjg0e5mHdLGLTDJdK5dtTPFgoagMk9k13OKZszlTOAEH94wt%2B6VxwY6pgHekcD7xdtdHB8MXzsSNQwu7huFyF036W1ut%2Bxq%2Fmqdig6Uibj11iREYxM1ZBCnXC6Z1ikwZ20PQqyH%2FfyynGUr4A5LLEMDyXhw9k0jjzmOROEkS2gt1sZS0ObsqAkrQam4AXhqINieimh1W7mZptFv07ltAYgCoEgLI8%2BB%2FPCtY4ega%2BqHMfVP%2FO1pubM7KOFr03o4V0soq6JDhCouDlDzH%2B%2B4dkqF&X-Amz-Signature=23520c05ac28cae17723343c08e08eb59bb9bc8d2bdeb286fdd4f24ec8c66bba&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

