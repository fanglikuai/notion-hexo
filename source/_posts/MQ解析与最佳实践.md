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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666Q35CO2U%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T050110Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD%2FtmNh7ATe7klgqw3VBy8x6oqhmaBhuFnpu8o2cQR%2BogIgPlrNgr41CKGWXn8yKMjFL%2FgIpRH5yqhG7PPNk7N94%2BAq%2FwMIbhAAGgw2Mzc0MjMxODM4MDUiDG%2FUpyCUADmzLiT4%2BCrcAw00xaX02SiqcUtsqnfwybrC5fV8n2UvCx8lzp7hnZfzta60fcX7FAQRoEELA0OU0rsG7%2BCd0TUCsLXf9Uaso6DuQdFl3mPVKnOnjvXiCuBP5wUSM5wRCbAdH1QLF89sIEYqcCVl73pVDpPXwEOddmK1zCfJKUmSn7X9Fa4IbIqz%2BpZsD2%2B18cTcTn%2FoWtrwf0pVUrgxcLRc1bB45egoRVGsh143zg5wSqLui3VGdpqA44SqobXKTsfibU6TiAwy3q07WpjNeVi8mVBvl8wTeahWXt12DLH6gU3sCk8p3k32Nosm%2FWdZ%2F5Vx%2BJwkiCenBwLXUP1488t5SMUcS8e455jBKzdHwa1YY5ymAl9F2LyCn2iuU7BPzvSw%2FXjgbDHUdBnks874ah0pdbymbts%2FoUEkS%2BcdH5f37AfUMv2H3q46YWkiWLR7CdoOic3cXu2knRzq8WJEoE8TDuFi6Ygr5s6gV%2BbfQnNiazcCRtpH7yTeFDUbFwi1vJwQsx1XA2yY3j%2FbpuoWXhWWWpOxKsjO7Kfm8KcbwB5Pl7cW%2BxN6Lgn3ErSYCoMhyE3WKJi0XiX%2FqMiDDumTdewUyg3dcM92S3ms9UFmXHuKM12VBIiv6DGgUoNkka5jH2RKRvc1MLCs8ccGOqUB8g5ceQ0RdRmvynwddbhUWBabx%2B5N%2Bfb0xjUDIFFsH1bRJl4vfQpWhwtJTIs01rmQ9PcglAbvT6OqALahZHOv7ZrTuDvdPiPRaw%2FPlhwu6s13SS6aMNk%2BU9yFb8YR3RgpxnylvGwZW3lJHMgcEZo9Z93iDMjZEVuzbzjxcule5WEyC02iaj6X%2BL%2FSRQEQF%2FUVEupvVud3O7KmvmqcbCSsHVrSZK2E&X-Amz-Signature=d25c0a02ae781c85efebea0fc38fd638e6e6148fff22fde831a36098f3863a2a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

