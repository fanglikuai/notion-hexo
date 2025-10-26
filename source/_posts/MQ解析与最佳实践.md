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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y3OS44A3%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T190043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBCerCjW3602lzhdyoVfxey6NPMlZW7Bbj04Af1csxqMAiAgWCq9UMAurNJ4KzfipAs%2BhF1A3jBhyihyTpDgxoMNliqIBAiT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMuywDyOOyPC0B2xChKtwDTbunwH1yQUkRWaJuPNUU7tKESqsk%2BzL3VkcHqZYz0XXES5yJFjzbVt%2FQqL5UomZizA5nJ6R0WS%2FYqMb2hAlpBkMVIOSvOAuxP1t4oROrk1f%2FyVvI%2B6EKTaoeH5G4SZE6%2BiGqDwchTlRlz0ZewaIETX6ldESr7iFig84O0o0Vw3%2Bh1YN5gcjHVhSkoc%2Faj5X3Dc63Vv7MlWmPuNrS6cR0zBECbNTr4IpOd0WnHnTq%2FfLEM2XKOOIPNAoFFzacKP1nli5JWCLonSsT43HohCF9AXqCntTIj1C4k6EO9ON2hfojJTbDWLA3XOi6ml8IRCeUAEs7qJIbv6%2BZufKCIpBQqqhFqEFwM4HivrCbZdARFS1CpTEB5UZnhIk%2BVX%2BE7zU4%2FeFXwtWct7HR6Sb52Q3BAYdWbOSQTsSd0vssz4cEJ7GVePYaq3Mbai%2F2N39JrjI8KMGO7CASpEehLZ5pR9fKhatSm%2BF7PO6aKSvuWvhIfY9IYyblYIxRDxQcs6kRM6fNIivRz6XpYpORlN%2Br0cLmR7qggSqdURsREbZZNd7D4C%2FWS6YwxFgz%2F21bYw6rY4IHbzS1zLPR1n%2FFcz2Zf4tV90Vs0CP0cg0s4mO3ZBos5j12Qoh%2BLSGDI5MHHM4w%2Fs75xwY6pgHPb%2Fk0X4kW8jwVsztVvSAaGZW6NmZVKbdvInMrJxioI%2BFYy%2Bx%2Fx3HIiiyE5BO3yQ1wd9b5tbJ3QpJOQva06Ir2z5wEKQaiDnGroYPyQTKPSDxr2a7HBbCoYtT7nNuBOpsr4yZ2pNEBFHxL%2F9cWwoZEtZYTLoW30JZP9fxCArJNxxffT%2BB%2BVmV%2BxF4odPIIbTHm%2Ba2UtO1%2F8nXPiDcx7xBlFQd%2FTGKQ&X-Amz-Signature=10145fff249aa7083b579c9b0347977d1946e78938bb4aebaaa58e168e859773&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

