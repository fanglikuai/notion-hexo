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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664VVG7IZ4%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T150052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJHMEUCIHrV6zfO1fruR8Dc0yskLzEOCARPDbDeoIZ83GFK2DcMAiEAq88S4gsZdY3Bxn1uM5rpXedXSWNXlO27fT6B%2FWKrPmoqiAQI1P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHibS7pCAv%2F3pHjJYCrcA0fceBNHDhu6hNO4tDIBykgzRONExPcxlJ38fHI2TLSRvKmUiId8VyyciNVPPI9GDm%2Fw4B6F7vl%2F0Enh2P2qlJnKURzCyAjF51ptD0Dt%2F4NO%2BT4ymQC2PUyD6ii0ZjQeD4RVn3trtZuyPs143r5jYpnmIZSFBUZIH94YS9ZJDCjvGLpwTEzGbCl5OMnwsebDeu7OheDD9l%2FILajZPlfDORSNPSDRaxkSpxEabOz8lnRRT0o3QegeEnI3z2gmY9ccJ6Fm6gyJ9Xc%2F4neIEt5aWh2VSVIK1XBKZ46I27lKjkay7X6fuiml5H5r%2B5R6fZ0ACBrFdn%2FhrRo5YoKEuubC%2Bbjqrzgk4Xam5jB38A0%2FqWZxX%2Ff6nmjq1mbqPW1XQQ38SaaqapOL9nEKhTGV6kQrHHwRzORaRe6QtONfPW7LjUSsXWE5mTbKHcdeMaCTfEYbNf8%2F4AwAAO1UusvYw0zaVNl17B6FI%2B4xDsIfQno9R2S2xSGzCFPTwY9GrnveBWegCaiOxb56V1q80wa8U%2BL86nLKeX5j751UZAuJ6cR%2B883ofPu8LoCec5Qj5qTT4HJNZGIgqrKbNlnKuZ%2BDWJiASCuP%2FiB45qOXIbWQrkt23x3y7VdpGVL1i8pemzISMNzetMYGOqUB%2FrAi%2B043It%2Fp14e2CEj2YjU%2BfaTeaXEl%2BTid6RKJsVIktYhIEHulT%2F3bht76VSiEmWRGlZHw4ePOsElDG6Ezj4KbqtnCRMtaC0E09%2Bc47MoGUbBsHXbiDckdn3Mh86Gwg4wgOvrysxD7dD4WvCyBk5C6dyZserurzxFJxZpHPRCZqybU4cq6hicTjwzn9ygwYuF2oGjp9pLK3Z2Jh7CTL2qGZyEh&X-Amz-Signature=0e3cf6180ced233791f89b6c0c8401b50738fa37c1d8458f243eeb279996f802&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

