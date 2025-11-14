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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S42TD4FE%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T120044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIB1AaoY8Wi6Xr5naSAWXVNHtbw0LPwUUOtB%2FcclkkEbtAiBfdfcsv%2FlHAMnIUc%2F%2FmZ9nJ9CRcdtKlOsE0aK3ygWfHCr%2FAwhkEAAaDDYzNzQyMzE4MzgwNSIMCgzO3dlNMOF68SMyKtwDKtzHn80G1hHieOgh7JJlaNLMEUV%2FK2v3DSZS%2BC0vt%2F0aLCf2GG0UzGrXfeYKk7dNRzIFebBBYFtEeDALzeyhuN6RmcZaOxfQFd1BYlFHSYi57qrDKwH5yjuBsZVwIBWl2FJ6ipx3eflDgEyF46wBSYeWSBcGK1lG1%2F%2BoA6IWkLC8%2B0jdIif%2Br2svAbCYzIkvR3ED%2FoFNpfKFp9ta1mpnHUcWGIvjaGjvNj0bRk59A84%2BprkqLobgA1nBmlfFAgtmTVPpb1nfNQRf0ETbkVTuiB5WriuTGf3UcOep2Q5TCHJfERff%2FtA3rbhuRDaa9r%2BpTrzsjYXZjr%2FXcDw4b2I1zyKxZ5xKRGD4oQR8S3Zn2WFh7tFMec8KMBHa10Z5uvQkWh1XMikLIk2DSq8JExfCE7cRj7wLVE6wBAFE9a2UoY4nzDGJaIVpb76sqjwmTiUJi%2Bs5HlcxYWpWFx08X33HCV%2B3nLX%2BpPHh0b8itBhUlo%2BC16mFOrCW0AQrhyr1nVyUC796auyotQTS9UGDqHxdPMShoXq7lHwVJ9AGxronHbeGpUA1bHdxBv4z4%2F7CoQJi9E8lG%2Feh7%2FUZmrsrxe0z1ygaOLAGjYZ%2Bv4llM7vc6E2pZP5%2BR%2BhA2XxLIGQwlp7cyAY6pgGJBrHTaVH0RQFJTLRW3cVbUlY46g42ICXbjsSUE4uhI7iU9yuZ5kEie1spg3uXXHQoIvPp0F5g5%2FPrpKADeua082mWCCoNONKYQTmjNyexJ5TkOLe%2B4S1Qx8aAj3P22wXPrUv7539BXYKyBF1pRn4%2Fs%2Bnbgqrw%2BAg6iNe9llvtElF3DvNXCCcMO1nr1rYdOaoKhOrauuXsot7%2BhA8grsU%2FXwyVXbNX&X-Amz-Signature=7fee146b412c5780426c3143509d7866a2a6e1234e8af4c9dce56cbabb3d0a2a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

