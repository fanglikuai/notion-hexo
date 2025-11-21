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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667B54U6GN%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T010043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDkaCXVzLXdlc3QtMiJHMEUCIEwZdC1d8oSdDak9ZaZ0LgHndlqY4p%2BFJPlTHIX0QtlJAiEAxRUGZLUapYhxsUrQaKKsT8ACgmK5eOIYHC26eQBhPRkq%2FwMIAhAAGgw2Mzc0MjMxODM4MDUiDDyxxu5Kf5lJ0TDuhCrcA%2F3fEISQ%2BRPQCyD0i2Z2akcyUncr2Vr5Kz3yWhYHMqYKxNVWmYkEbnR61uPu7myatfi6sTzJBASkzDv8ZcovNPmDsuX9mD2FUvwSrMi8NL2CMy1YqzMFbfwP2LKTyJQiDQ9qqNfR68Xpu4PwnsEKiqMYwCoSZDIpwgNWiPu4aqOIvJsCLuIXwPzk1VWNWGOBMRVxg9nDwTdaXDFIRTX2pnzGvX0floKa9o0qsckOI8qNjh0G9YwqwfDUqrDkvLj55M1WguKKtQ5MXe5flsERmFpc5aNJN50RKwJhpEiJLX4ymcwFf2uzy1XdzbOSF4i61zcF2FZCKnGmag8YXMTDZw%2BdixXQk%2B%2BQx9DC7TC8h31bKgSw1FWyj44OmMhQAMeGVR%2BWIoTDAx%2BOWMfy1AbzD9AqwvJvAf5Wpyl5YsypNN8CZmW0za8TIi%2FQANIxs6QnIjZrqYM3qh4xoY8bD3e9YfAFseodE05PvGUUAScOra6iSC%2Fc%2F9uxg4G%2BLArv7TraTsqSjcBv9%2BHHHN64RNgo9sUl4TL4S%2BFRdhV7sUpMmY%2F2bMNhWkF2iuTvEgm50oTBXnGBJbE3lNmNZNUQN9ZVOnG10KoA%2Bjoi7%2BCgq18c%2BZVOXQG2ZoHU0uwfoJTDMMPp%2FsgGOqUBiPBV8F0NboaX3SnDcJnkvkQn0YpxHy9f9k9f3cjLsKEubKhVwhdaE4hJNknH7vwSSlHUkh4YPH%2Fgq1BeHIbPBYzwsGVSLq3LUd4V78a6FsR0DnpDdWbfjJBRv1fVtCGXGZK0HUa%2BeJhwWP3nH%2BDjYyOrnyyEz7XWqtWSZO11xJwkRj9ZCKml7CDs1rVWpjASui2PWslbXUDF2q6Im11ImDuWiJTK&X-Amz-Signature=e55d83a9920836699e449525f3ab421a9e86f537ac24824221b4657576c43a09&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

