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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666JNHQ3GJ%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T190038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCxtjXUBswgNGXkHo7XPH7Xkm%2BL6GnLU7fnpN%2BlFhFwMgIgarFcXfwyYfhUGy5Px3Q6u6YonBuBu0T81eBoU%2FO67ecqiAQInv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLuVaFBv5%2BHaqag4YyrcA%2FIpvjGJXFoCIMMBkK5iDYXoeAH3anShNJ2zfQk8oq%2BGyj1SegVqnoZTV3Y5V%2BNqElOrI0%2FIXq7Xz%2FBcSDcvMAdzs3uzEWZIfsQeolVcXAoQfuSqkYMm7%2FZppTlKI%2FH5MfmkrWptf%2FJxFskso09t7CiuI%2Fa810fOvEeMyWFTlXRmP5uohgSLH2RDD5slqK6jG77jm8gvJv37%2FRUJCL29YWsYs7%2Bue%2FlcU51xb1nuSU6jT9mE0h%2FzwllLsUpVdedjcx7k76aW7MYrQLOCmYpaQ0quUT41JlsH5cHnWPeqh1M0x9oA6i5cAkwW2576yd0BOsC8qZirkBQSmbj8%2FI0hZVXSYvbVSIyu8wbp75aWhqpb64FzN%2B36hmF6UyFC6zCvO%2F7rnun0R5DSXX4W%2F7g9xFh%2BeuXD9Aa3NSwM2EO%2FIB8jGN%2F61nQG%2Fc%2FFqHDXZkIKjl3iBS3xAM3uP68OR4kzpk2Tovkhki0KwjiaodK0u6UXtf0wnMYskUpmnEJKXgzwOwhmPy958RzJ95KASmJPUebwlOMgJuWMIyBW9ZeNHxqa3%2F8OMhmLbkE3XiV5E1ogjLa7dY%2FJfXD4vK4ukEkruGbsRpENCk8cR8LQMdCj4XSIDlkLAtg9gEFb7p9LMKaiockGOqUBgwxvwY0F6fbUWOitRv3E3AdcBlifUoSEu%2FToEPxE2e8KUhMsRbv7oxZya%2FJ7fj3es8C8Ca3fqAJSaDI%2Fdzp4RfaCs8jG64w9g7F4t026Iwuva1oiKSytNOpsVUxZlkrVsuCCqq5dQq9%2FRjzQM3uQnNVpzcD4gky9NPJWqX3%2FzmFX2tkDNt2T9geWioXPFGR3hkKgOcTAUSx7yDO9qWHDCWTzHvdA&X-Amz-Signature=2e549ecae11789ea52fd0992c8d9c45c31b85bfd93cf0ef3786c1cabb2c981d6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

