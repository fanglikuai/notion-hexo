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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UTUBM77G%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T160044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBAaCXVzLXdlc3QtMiJIMEYCIQCM%2FI9ARE%2FZJ5GH5IeNcSJpj1OD1ILnmfnc%2FOgVyhS1EwIhALqgP%2BOPlys1%2Fa9GIG01CiWFUMI%2Feh1iBHGCUsgSn1C6KogECKj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igyh4byXz1JT%2FsErF4Yq3AMbMucf7DsR%2BmzFJ%2BPI%2FymjGH%2FpXsadKuXBEiR7N4%2BgB1HkRfg4lhlC5VuYH9D4AmTh3XWLX%2F3JPj9N7erRHKh5IqDkCvVJt80wM0jjwJmIDlh0cRnZLg4hIZJs%2BAysT81j%2FAm6pxYCgauOUe3%2BKtlQlGzw2oh3CLm4o40bF7ZxzSjN1ZEwHMkEkDT%2B0bvvO%2FbLaUJXyBeLOqB%2Bga6UoSl6EucrXxHEG87TG2x%2BpDYtkyukeMF35rgdxJHCdNBT6lZvbk0Vh6%2B4uBiHqw94bu7pMW34DtreeR7bLSwU51bcitq2R0HI%2FsxLF7q7SiGQDQrKvJj%2FkaH3Z0OVWIASltIT0gP2z9ec2%2FpXikGGT8CKZoA3%2B5j%2BpWaRt6EWQWk%2F0gtAK8VF5JQgResfcoe8gxLgt2p6W1Qik5yxrxa3cDqLWib6G6OY5uRxOHUGTc7G3r6rU%2BY5%2FMSXaALI1ZZcVTu2SaQyLfCwVfJ4bMY599jTohp0d7O5VuGZZ7N9Nfj3vS0PYLzb2aCctA64XVYLXrKpyBOiD59BaSG1ZgYxpqjNjG8EuuRWuyofLeDSaVTuTmBzAQ5m92IdnCywJmlGl0kjTi3JbzRz27tGJ1lFVmWXPZTaF0VEIb%2FKobD7mzDC4ZTHBjqkAYLhxssHO0SAHzFkUwu3DaohLR0A%2BDyTBmTXEnGtLBsIDjnMb47Weg998CDXurYeTRJe3JalazTltCZxGF22xpqBqdfoIcOskNpOPBA9CEE6LGowGShZu2oiGBGSPE9U8dBRi2r4PPb21u3YAEvYjbgSEMaNvRfob02JFdMqI7hbpfAeLTLhKm0URm3LRU2HJ7fJkyJxeSqjCwYe8ZcJb%2FORSZt0&X-Amz-Signature=4a153a6a7feb9b520fd550042f1ccd2230e039933220e2d95710e9741af66989&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

