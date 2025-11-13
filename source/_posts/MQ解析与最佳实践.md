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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YEBHQYJD%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T140107Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDfvMR5C0BvS5U0r4KL18UHYkLo67sDOAESSLDWCBiDNgIhAPJ%2Bmddg9gSIP4G3RYy8XZ9c%2BCzPq3v8fbnjrq%2BVPO2PKv8DCE8QABoMNjM3NDIzMTgzODA1IgycMiiFDDjJ9QwD3%2FEq3APxCbe6bsDVCotr8a%2FbNUqm%2BDByKeP%2BHYCtrqVn2GBmYyqv2M0Siza08KYtxYSD2cTuiR7kn%2BgO4eKUvgP4Z318WdAvOuRxFfPYmnttXRWKtBUmQQtahwVYEdX4LogV6zJmkdjhyxU5rj%2FyqN4ywbVV3aWpZ6zmhv0YptFTevkgYL4tRU0oUlFOO4y5oTI%2BNXRTVr6LPAtYp%2FDSE1i%2BdRsc5PLWTrUAa7yAxJ2mzOueYb382Gv2b%2Fia59q87rQF3GDSQr0a%2Bp1ABIKpOQ2cxe3OG5GRdZxm6Ulp5LJXJd%2F2AnT8UTdidE6a%2BKwY50CeTq0VuBbNpE9nNqyJbxYSwMhucSVig0HgJjgNJpN8ICLeXQd7fv2d5F3OhpPpcaJGat68daBgBaqdPtzvVsBeSIeiNGSn77rsDc9LzTCRX13%2F7ExGnk7PsXwVFDPTJzM1NZrVWRCRecR1263ro3cD70QkmrJ1QSm1rTQCi3Jr8eZ741XMEhmR4lX%2FMDx7iS65kZLaJ%2BxqOIDIXLtdNX%2FIi9Lgy7Id0Ls1PAjndrb1Sdw9jmqOmNzUlhlEOMOXMnWm6%2BC8rP%2FaJDfyiqPU310O137XHJ2Zm%2FkdL25BLTg7DMhbQnkRYMqyVZMCuB%2BjfTD%2FwtfIBjqkASMjUQn%2B6Z4iYTbmisJ%2FFOgDCIOSDTo0OlBHDNGPQG09Hd6Yc2lgdBnMQ0sUHF0WA5MZRyRRytRqtmHgNjkFpVcm4rFvtzGeYJ2wLVHEK%2Bpd3pZjNJg7F8U1f1z%2BwOMmWFtNOP3wYQXxvHRZ2AZFR038pnAKir4IOFzS48IziXNyb6J1MU43ivcame01XpVPqsYXLNhMK9RjJfj4HCDhQRP189ZH&X-Amz-Signature=ab76ca2ade183cf170f76e210e33e66ee7cc49adda32b0997cc96efc6db8c4da&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

