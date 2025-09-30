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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662Z7NKKGY%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T200038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGsaCXVzLXdlc3QtMiJHMEUCIGBvRQTFT2e2nv2Aiefz8EBwLFNzsG8n9mTobK4msEq4AiEA7B4p8PrnETzR%2F%2Bq3K1dQKGpHTfz7ZtybCskWBcfd4DUqiAQI9P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEKYJP8BSu%2F0e9uzXCrcA9iMdH%2FCFk7YL8pmVpD%2FudfIo%2Fe3gbT9bOI4yInPi1oJoiSAHg0RDCc95cuORd58qRs7NFtCnofqIfKofvRKvwcNfqmfcbdu6%2FTzi1DGDr5A76aVfD72AUiW4WO3K%2ByNXoroy6argxMVQt5CKj43R7%2BB4ww%2B8ThYEUTFMz%2BQlxKYtFmdsb1a%2FRrDUvhMZ%2FB8D5QbLEfTHBEkir53fX7I1LaiteduTR4ee5EHWp4Av1GodD7pC7BvKsxcH2ajJ414R%2F01BDpYmyOw%2Bh9uKfu%2FJZHFn%2BIQ7HvfbDDKuJLe5RHt7uv8g2zJZiA%2Bhw%2BBdygLLYTr5KZpzy4R0anZPglRmr0eKWutBwf%2BT7sc8leJHja4iR6nB%2B4BqO7akA5JEuauZ%2Bl72sZtaZ7wjAQSOjk5HzGSd0wjMeT0zx7ydubV9yzwXv7ylyl4AN%2B1pGS2nqKn7Q1DsdmUnzH7Mfj39M0FrYgrx8u9wLSVKfXpVrNkL%2FgsKB0RnH%2BheLhbyRF4XsE768zFixp6hTuIrbUynHFa1WaJCemr8NVGqiXDTPKQ382svAf1%2BOxl1drJ3cz5eieqPRz2W02FnIC0FCh64yKNoMZgIr%2FrjAHvFXBc5AU7B%2FIfpK3RGQUJ5MBRj0z1MMbY8MYGOqUBZjSeTDJImOJ6gHeXHChAntw2DCPwUL2%2BYq02GOkb7uYxp2uEcVJs042gRbeaZzMeZZlwnZk3XiVfvCDaMv11GVzTpB8mob9aLxwVCi7omeLMf8y7WGInYaGtN5KyJ9u1mm3vczROJBPXfgFfH4F5U6loq1dUohZte1BCs%2BxWPH80c3d2W%2BOIEAQRgCKtSvgIDDRVbmrPkKNV4xVxCPByo6J1t%2BRR&X-Amz-Signature=e72ca9e08b192456e3d7ecd456f637a26492d5f49b3224a3ab0f2fc8df019d29&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

