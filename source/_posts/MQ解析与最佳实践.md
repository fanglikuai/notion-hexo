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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QYCZQ3KY%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T220046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDMaCXVzLXdlc3QtMiJGMEQCIHLEfsEqsgT8Tn6AiGICjOWoRraZ6JnlUfqIuO3ohVunAiA6o0qNgUlohB51H7p%2B4NMa7QZwdOjJgT%2B10TM0RoO24iqIBAjb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMgb7R8LFo2%2FpVJceiKtwDEsHPH5swfTDasqy88FRPmTeYFBvudKs1sEdzTBA4t%2BYbCbjxEIce22BDkMRBKx8oTbyZ%2FXl9sDYelBLx5CWdG4Rj8CX2QtosrsOTg7KL%2BhSvGyCbBeHIJ6DC1BeficgX0dBQA40olskK6PTV%2BHq2UtVLpWczMuk5VVb54fMeAfTvz2GCWG8b6J72N%2FH2sTMpWqokQRg%2FbBJoK9naNDp1hEd%2FKFYcDfbbXcCR2%2FS5gkWB%2B9pa3293s7HKnV%2FJhXpp1DPiMeQRD51jZuJjLKGboMlXwobRtAbJd0mF%2FA5%2BjKPLTiOwfgCig%2BMB6RDN4sT87%2FIS%2B0R8F%2FzCp6QD7LlnnHhkF9AqqGUGyqMViGwrCciKaK8%2FW0KtEAv7br2yqiGFxushc0c%2BY44ANzmc0kQDfpURpP5TUi7UGGU0UwOyYkQXk%2FVeioX6LrsJPYA996FPy5Wo%2BZibEVojPY3Tclo8zTkdWm%2BQKYTjgSgpKq7uESHu7fDKJBgi7FWmM54%2BNLePJf5larIQajPqqa9WjZg%2FReqAoKFKYJtuSwhnIeWHZvIv5xl5y4inRfwrzmR7Gdyx80prV6MKs%2FEJ2ErWFHGtS5OD5S0GTjm1viL02Bk4JllA77kfNipiDLeP458w4tbUxwY6pgEST9sQsNrc3VMnyUPrnzqBEs8YtcLCB7F3DJg5D7YEqSDFW2filhykcNXciJE%2BBRI4z8JtfQb2COOWmgC3v8f5hbPxV11nI0dr%2FSPkOiJhC29EVzAZ00TV%2FFfjtpUgrcJ7F9FT8idjTA96GHvClbLhgeSKG1h3CiT4yVT1yZOO6h0WfrgbOC8nsxpMZZemxbT9%2Fb4WJoOGJFXKlqdSdY3es40asLMU&X-Amz-Signature=e193f7b87dc2cbf3bb92b4b6d279a38dceec1a3c65e6990fa51a3b94a8282135&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

