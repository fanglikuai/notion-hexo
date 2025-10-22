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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XMXZC5WI%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T050044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJIMEYCIQCVTjAPSHLMxsnmnRESPmL3BjlbLPnr3gZicVmn6ZRVnQIhAP3K8K0eHb7GjKT7KjxuKdtxA%2BwgexOyOvm72pIKXVqGKv8DCCMQABoMNjM3NDIzMTgzODA1IgxgdobfKF4bPusJGwcq3AO7ONhKYHM6NHn%2FZskcijQ6hJLc0rWfPnRDSIYPzGwqXjwq%2FIT7JYAaiI1on31Fe0n4EU3wtnHaCI6MeIgP4WOs%2B2FwoR5xpxvLIweGK73h1EKmeclydniU8Lrvzvq1M%2B3ZOnLRPrHGvEcJyvRVMeV0EUAMpvXdI8BGoww2DvZU2qVAGh7Af66JsATOHFcilrzMwbF3RE6jugp6OOWWMX7BOaCJUoVDbXbFbT7ss5psx1Cxg8Fy3JSZTjICcFR6FXo1Ui2qTPpS95dC8m9lJTnB4oDKHI%2F6W6dWsfONZWkXuxdPP62Lonw9uLfdCuzGLlRzTsVA8Ge2FIkFHRmze12L1fqHexFNIvkFwILsA8Y3N%2BDjrRw3Hf8N9xBgrLKg8Kf8k5dA1ISunNfr7OWMnYD8oUgguKhJEONSXA9xLkq2%2BioKYG63cuo58aXFPVH6FkkgIMUjCzG34tsTCrjiScm2L1hXXJ7AldmmfLhJwEZ7htlYaPdZHc0NUC0UW4vzEsmslAp%2B3nPydZ2gfh3CXzrKUEAYXD5F79nBFHBncUoVeZunvfSfPKNYXShadlRtwmoQve5mx5e3hiOpvABSUoAY2Lm3cIg2F3E4e3FD48N9K3KpvrTPEF%2F%2FyUTpsDCl6eDHBjqkAbdnDUQ%2BpuR0Mmo0O%2FNB8Sf3Md7kaHBvfF6mi%2ByoiCrk1J2MBk5XE7lMiYb6VYscZI%2B2SH6P%2FIOojz5sp5fhaXGKWAnjigvC%2F1h8HJaWUiUSl22swgCgfCiQdhHD6bJEbgNmlalGO6JfBZGBsBfw9M6z5CfuIK9%2F7mHd3695SVgYyJncJaEhbH2Ofyy6MzQ%2BZqF6jj90DgTHEDVeLTED%2Bwqc7LB4&X-Amz-Signature=146aaae7e578e77017822740b32e6e7bdcaf5b040e3385bd79a39936e3832f7e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

