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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V2SJQ56K%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T010040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFmXuT1bSKbQQRV01cHKHNMynluxothAb%2BwhNDaknGPUAiEAtpLdvuMh%2F7%2BLJ8qBrHoeOnX9Q%2BKYxetoSwtHVJct92MqiAQIuv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKfpeLcuGC1FsWgZzyrcA9qNcl9IcfVVHD%2BUXi2CoSfJpxAZssW%2FEHYNhpqHkQiALt99fdpov9VOPb19n5p%2FujIEiWVraTyDda8o0BVrfsLQ%2Fr3aaXeX1W8fGkN33I%2FUzAd%2B9qgBZtZ1zxm1xHOdyxm6I%2Fxe5P9dGgleXYMNKMIQyMA10%2F%2BoFWHC2xyIN1XfvEcacNHfYDkZB5Tzcv7gFdjrJUWbbVc%2FAUIPGKMTyaJtCxbzOdJXOCavN%2BbR9KbJHvThwypdkZ%2F5D3Og%2BV%2BGmO3YHGi6dC0ra3LTmUcBQYRr6UO%2BREw0K9RIHN7BdXkmwD8YZyKCx6qyYdoiTOpIfGSEnDOlo4Nm5g7zMs%2B2UihAfa6IvqELG8py%2BOFmaJX0yD9icCkXIIQLUzDenElz9fjgCq%2FW%2BLqY6h25yxWQyqgWGJPR8t7EIImgPgaMJdTd3xQLyq6aI4WwxhoDelUGORIOjDBBbFOHtDYmu1DgcP6Sdji%2FBT79fwNyHp02YqS1WH9%2BgAP6Kxt6yo6MItJ798kYEETpa890peoK6zMr1tH%2BZ4xGUTa48DdlRf0RJuVKqFxEexvEvjskfLKgTtv8zayI8UF14iPMxxurxzrBg1sZUXWFHyxfThhGoMSsmaWRXn4GHJqsD0MNCgxUMLyA78gGOqUB8SkpVITrSGW%2BQG3WyEmJusUJ2LcBwjowMJLjod5RscLIbyim%2BL%2BgVrIv7GM4H%2F5WHkk7M2sj2ahUp9xtPgqozvhuBkyp4D7%2FTDrfR2YOydTgKxJ8oBmSeBsoTXi25Wv2yPHzF4XvzvByVBfFnVijJ6d7vwTzGv0RySTXKav9KZHtvTRTymInqqi%2B7yFVwHPkUu8lrr%2BolCpj5tCM9vDyEZGE0Uwy&X-Amz-Signature=e5249ff0f70cf4057a1b07e11a8604cc9515ec949fed9ec8ca64d6bcb595aff9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

