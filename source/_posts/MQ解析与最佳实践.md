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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667KKU2XKV%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T230041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG4aCXVzLXdlc3QtMiJIMEYCIQC3FSfc9FqPi7eDUNh7hZWsSP6TcOPsAIboaaiKc%2BLyogIhANat2kpTdq%2F22ePWZ08nWfcRwZCoTySX%2FK51Q%2FE0bq3fKogECPf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyRYxcfRjWzn2Aw8zkq3AMgEno%2BtGZPPLNNxiD%2Bi5tfbMHrVkHvW8qqWWOUTehkUnsW%2Bhu2Mj%2FJXoPARRfzp6rjIVcJvJAXJTzalcHP52%2Bz%2F5w5DcFtdMwBQSGEMsbrz6Opksp55ehYZU2Yw8ei4seE3TRoSjPlVphLAP0iIhZ69V8EaOanR21DSRbK61hfWU%2F30vCgxYVuXBdtNzhI9DXDOEY%2FCqcNukoRSuoNfPNuI5wUAr4re7r9M6vbiG43J2wLT7ytXtJ%2FuU2zLzQBzAhZqWp962J6Po6%2FNoxie%2F2msKlbsaIGBKqyvl2cDHkloJ3hYV6BFGipXDv4zT%2FLVPEi9VGAKfMynRUQsxZmHG2XOic2Eu9WLeob6Z%2B7%2BswTRLrnH6WVeEuMzmLb7fHKtoJrsLemCeeqh8JtcDpUzoz9yEYf%2FM%2FYTY86hLVmVaBL8R%2BEXueQK0fDgsBbEcJYm6bb%2B9vX%2BZ8LiEY7VZslKDe3pdxKJ058csPfmXESmNPKw4ctY1BHxtjiroxE8rHSWi%2Fz3IOPYX6UxrosQZrpvH7K5sjFxIl%2BK%2FaJMGb7IBeR%2FgArUrq7tIf49UstnpfDIktR3b2%2ByNV4ARU46xyBdcA0Czx4Y0pU5Kkgkmmn54b9osqF3rTtixg3Xc%2BWwTDVnvHGBjqkAcvh4tXIjikCeoE53jgX7nKPr%2BRHFHEKOWx04t%2BjFIXxiw92teR%2FDF1qrlodhOvJkRfpNRG5Yk8Vhbzf42XdpAqFp7OUljxuvJXEbtsrg%2B6vWXXwFhDKX9NR0SKwUUfKXpYrRPL2djbN2Kvk2geDJEZGfm8Kr65JGHHUiRi%2BzreYfPQcjPqmT8JGaQ6e09YG0C%2F7ulkm4FdGzVLS0RDYHTZ2CRCc&X-Amz-Signature=fc965bbff42c0427ad6d019efb320ccf9b37b875c12c4723bd49d75fb8052037&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

