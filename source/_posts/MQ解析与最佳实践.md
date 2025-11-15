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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RY2ECVE2%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T170058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDAsENdkW1uJIuGoqLdw8w%2Bypt7CSCroYFqhCmpDe5taAIhAJ%2FE5GrCRGvMrCh2ZVADa0zSpldjCo8mfSNb6wcdo6N1KogECID%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz%2BZi2FOtzfae8Bhl0q3AMcHGS%2BsXyKmxEvns3Lk6f%2BEJDAPbWZ1QRgMJ%2BOTd1fkejGF4K6O7c4syOV%2B8QJCw9cjKis6d7b5PzcJUrkK3mDVr05xYPZmYKehUKPqetzrw7cITAl9MIpyPBJshRK5qdWf6r71RWZH1LSO41K1rHWJtZcu5yCQkAHdjeb%2B%2B3xZfefGeQYoNl%2Bb2aHM59bBiFjdcteFsqioDQtpqdqoSLw3Z3BI8dl8HlgHuoGERgAthwMNpdqnx0pBmZnUldefzuwJly05nJ%2FOVQwzCwCV6Xlu3mPL1Gqvw2922eWB%2Fa8wUAteYE6pzeVtZOnDiSlH7DgPI98JMUmhMKn%2FLDzjZhba75aXhxjq0yFE819lQOia1Uk2NITM9%2BaO4lei%2Bx96sM1V6yLFoXBVYfPWgubbisAF0HSKBfldcInO1%2Fsggw6FLgQQ8Y56%2BgbUwdBDQANm6A1Mn%2FYeYuanPZA%2F2nPRbIlt%2FgiJSS4aedSfh5Vabt81jf%2BYn3Uqy5%2F%2BqRuL%2BIput3QUr2Or8ZvBs3VU4eV1CJ%2FTKoPS47W1dsoYmMJ5%2FXZb2Hnmr%2FYsGRh5k1nKGno75pLpTZky2xlggQX5ep40Hr4iVxtK19RkSPZLxdoiNCQ351Nrq8uFw9ggQEC4DDvn%2BLIBjqkAR47FanKyIIsCD9xXclv6Vp%2FzLECifw3VlGI9xMQyyjtx8SsLj2VrKg6tdJ3wEz2a3zF%2FNVuafoTR0f12x%2BtvU1IDJ8TbZkCsmeH18yOwHinzefgcC6um7bmTgmXKKQvtcp1N1ZUXz6CPhF5yyiBg6pt22PCDIalZHzufC9HZlxHH3%2BEidmAjz5eXoTctgwKawrq1QImAUtxkZOLxZ25SIxogSds&X-Amz-Signature=0798b6951c505efacd7b99b69dfaacc72445f90efa3bf680e41fb12146fd56b1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

