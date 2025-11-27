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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RCRILRAY%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T230038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC3KSTQ6pOCVFvSdOu2dFIbX9lixFwGbJTgQmTqUY2g0wIhAKxWVc41TC%2FTdcgutkHWzaoIxL1IeBsmV7EgI%2FPqm1zvKogECKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy2MVrzCCl2Jo05vjIq3AOARuyLm9NBksgAvBjWUffoPdwVk8%2FyVsbk8%2BOsEE3yA5QTGGNY%2ByB7wjuDxUKr9soRlxsFESgeH4LBbf%2BNbyTh0xR%2FUjsao5zy8MRZi%2FG3C5rb29VSfJzJTjZ9IujM8ch%2B5%2FoaZKS%2FIEKjvmpfdVjfx%2FcLsF7cXETwqXay9yJzjL3rRA9CwOsihJxcf4oVPgNHGK1AX%2B7ISHL64QbAz4LuXPeWxhAx0KYiCfOOcLYgt7l6qqJKOcJLsDZkyYssj32LjMQaF23y47W6jtX4PcJQUavDk8yhAYT%2B5r5beKjhUcGDG3KU%2BwGZXYX62Qji2Y4osOd7znjN3ctaEK8yOgIpCFIG8lfJeRgbjMYk0uMdpQvkl72WSxr7BASMkx2oahgAx4hI%2BtvxFgp9QRPvxmqLGeu%2BhMtvfAg6TOgbCuCRJOahjmY2zyuP8VnOlW6Hh57TGGpLaDEF9rPtPDbw8I%2FyASGXkouBKurkGILT7bLH%2FeYv1HdVhFZPs9s%2BoFPXHgCatXQB79WDhuCGLNfGDGTIzJIFGeLcvxCu3gznpjnTHIWvUs22J4IvQoJZQQ7TO7DlX7P8WqpGwQLGv7Y5yAq18SHeCBBrmCIZBnosFKDdPOk2rci1E5xaptp1szD8wqLJBjqkARcWeUT4zWmXim795npL%2FVOcZsr%2BW0WDivtQyqA4x6iWiOZ3Rlgfan4r2cmTNyLAWesUuufpWkc1I3VyHkHV1iPI1InaKHHjH98UEohXxwV1htA4IyyKtbB%2FBSSsjjWJth6iBucprqBS8u3uP7M4cddAeoGLJfY7DucU1Ih3PvMndxWZYJc2GDQa2daBlWiMWxQJE0QsS%2Bo%2FwsznbiXhskEhcyPj&X-Amz-Signature=9f3ec2f441cea39b21509a17547fc438983da829a26cf7438f9186acc3b95fdb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

