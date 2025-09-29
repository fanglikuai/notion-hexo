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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UJCNDLKI%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T140126Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE0aCXVzLXdlc3QtMiJGMEQCIH87l4k9CBKtzNVYIZrkg%2FYjJO7NK4ymVJBIPn41SNOzAiANH4yPHCygXpOlzk%2B%2FIEntvQBTqcT%2F2cLR7%2F9grIIkVCqIBAjW%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMOW3qRN%2BCzz6h3MHUKtwDmi9JTO%2FhBrz%2FLqQ7NIakWLVSVBBXBQl35WPya23hXIzTSYmwjFEzXSZ81luOQIDXdywORUQhfXPoqY3XLURYqLaBqyKe6FpCqfNyqqQGmSUXatvzkFSnycjh4pZZBnvfQpzUaCWkmHNFm8FCNYk4Fo8W0duPi55ezxkd5C5rKJJ7I4bxxycv7MGpiGkNcVj2Wxt%2BRx%2FGXpwQC9CYb0npGiZYdGq8zA8nKuFMsNHO1XgnDDpnsX9HWNO0rgSb2snsxNwAtaom4EC10VJ4Tw%2FMdYqMr3Vejk4YDxEbB3o3H9A7iu9TYxWSPVqdpf2J8QBw13hKYf5J0d3Tuyjhv7PHRqbr6hkhIwHK4h9J2Nr3KM%2BPBDjpw1YMfNF3A%2FvFGsbFP7c0IsM5kXmRzQEoft3klq6fhDdJTks%2FDvuvrIekR6FqT6kP7DImv632PeBVtNTWwzK9tuiOOfuT4WcInTsgeWg4BSafKmlyh1ekpDnSGFcMi5DbS8uNTXUD6X2RM56SYeZWgkW92iN26kn99c8FfbZ1PmnlIa3uX0%2FYww%2BKMyEg%2F0A0KzJYDigO8wy1jzQ5pn7%2FvoA62zkPeVugfq4h%2Bdv18IEyUfHQUfCgPtR%2FySr6DZdqeRyVAhW4hrkwyYXqxgY6pgFJ7XAbqKFag2Dhp25ht%2Ba98bDWfYPjKRvL%2BhMeNlnU6mJhc5Eznqk5vq0OU63b%2FdwtQzBWNta6nopL3Ht%2BADSfYalPiLNxWUxXRr%2F90I9fXK1maJjGRBNnYMGeQSdfKtJZ2DMyfpkiS%2BX1zQxNPTCd5PhTqpJESgRBS55YmzkCwNEeKfkQMxSn4nj3FLxm%2FlUoMpcBi0pkOG8ge%2FertIoZ31QN%2FW5j&X-Amz-Signature=ce618bf308b8cea299e2ec21fa170355fa2824c8b457cca196025687d75a670a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

