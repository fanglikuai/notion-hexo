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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667EKL3THB%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T210115Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDE%2BhBhlEyt8GEvijMxuaLp0RAUeUXTZlA8BkDkZVgLzAiAVJ3VzfRSUTehd9oSkh7LVoFWrcz8azHpOiL2j%2BHkMdCr%2FAwh7EAAaDDYzNzQyMzE4MzgwNSIMO5WzsoDSkdRfJlGOKtwDxWXNimbqjxNmu8hrJbVi6xoAGLfWhj%2BuxIDwGGNVCxWrt5kg9QuowXhf%2FcXuDRyZUe2O2Xa6tU6kAikGXSSw5fvDv7PEE6%2Brpd7Dnh20KUpfovk5yjcSJGaLJ08Pxsk2kouXz1SudyPq4YQ0Q0Fr7FQ0y55HzjtmpSAYF67DkXZG1xlyNz0ArIGJ%2Fqwlw2%2B311vF1bzZwysjuuA3SPvddCgSyOQQsOGdYPPWGcBmb%2FaGKutu6SfZDivGj2ORalZtrOSvjterLYG%2FUkS9C8VZuxGLsw3QdjXXZe22Ue01WwKj9yR%2F21NTG%2F1Keam1NSOYiDQBAaNYjQ92m20RHB2kzNdX5cT2LMR7qs%2FX9JdtSip6OLMAmerEQd%2B2Elf0HlBteP0YzDAnmicfxpr3Td4Jzow2791niW4itniXb4VeIz3%2Bu5jatGi%2BodbN5uNEB76VReNBLRlUR6HJR6kv8bun9%2BkDQPfgrcBnpOkgEH6ruYe3trSEeUxwWjMPTe0hHCClSj3q12lI5Wyd7tsTOIFKpt2wW%2Bxj49BSrUsEJROxnQIrN22JTRAgp%2FH%2BP6snMHsvcMjsXDdanLkLJXyzqiCvVBmiHIbACP4Cx55Vaway8b6WeqIPsAU4rbuitXQwzOiKxwY6pgHmne1EJkJoS9hexm%2BOTU1%2BTeQNLCg1CrROyD9XbmwhvKpMUVDGt1QjkhwL%2BFaCwAEOGNuNKzwXRXSrjFzZ5ZLOZfP963PmH3n2yFM3Ohv4AR%2BsNCIwAU0KhkHjiOomB6T2Wg4cjJ3jUmTlBffLGZChNOVNj3UneAwJNACu%2FW9gSliaI%2F191iNlaAGZFU9ilQ8k4SvIxi6pj0U%2FDLkKY01zNm1xHR0%2F&X-Amz-Signature=0951773d7f1a7ae2ad9059150eb9c1937549d7290115f788b4603b3bfe6ba85b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

