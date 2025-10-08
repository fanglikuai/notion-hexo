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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VIOEXYXD%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T210039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECwaCXVzLXdlc3QtMiJGMEQCIGo5GcOlvcpycU0bwpmqmjFLID7GoVf4XZUVKOfKjepnAiA362QYBPZ7VDF1dmGn7O69wE4CzT%2FLTDd37XTE0%2FbgUyqIBAjF%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMGx2i3V74isyz%2FwR%2BKtwDziW9samd88eJPLlR%2FqSvmXoUXlsKhVbu6gcQvM8gURoDYxeE%2BJLWGa15qnKiOLy%2FroYKj9WZh1rH6c%2BqjqzdTZoukKmFQ0VofwOh1omWdO3xbXGIfy%2FwffXWcD8%2BYqyT8oMYfaaZSTtO%2B8p8GGPZgYyWBuhjHYCjjMFlN0qjmDtyJvXbU9ru7hn5Xc%2FJ1bITxuEXBZcQTOm48Z77RSQnZT2wTFxKCrp3r%2F9QD0RhJqIT%2Bfd5oHRmLd3wJ%2BEEf6b6yjJ2orwLjRB0j4%2FD6FXlklluLCq%2FJLeCFZf%2F7ZipqIOgtFWrkDiuoKoAfKYEu4FNkWGvz7xNDQ2qgz5ZXFTJ52a5bJxMr6%2FpHN8PglSSDSZeoxY7IVERuI74DOmBBQEKHMshk4pZEUg0cul%2BZJp4gEwszkPU6UHR4%2BXo6F5DpGYEGoXrATOhe9sTiuaa%2BOS3pfVEIfPM34tSGvYrF6FBSQt1zDd%2BIm8zw3En6Q243S22Tz%2FT6r8lHQIHtTpP8ZFESdJGjxpUrxsBjGmlgMZ54HPEOp2JGZQgaSo17rFWasrbDaO0%2BJnEbwFntKBjMiD02yHOXuWnOl%2F6K%2FMC9wzrY0bf%2BU%2FFiqovq5Z4Nu3iwuZ8iBujVW1mqUHFg5Uw3YCbxwY6pgHEOYkju01baLXMC3toEhX5VushjcSz4sccEKpJqT3qor%2FJyfe6jdvAqlRv7e6dcal1HvM3PPg9WMFQ4IWxNARj2VFggAXKpDsKxOn%2BU2%2BEsX0bv5wt%2BReZBYtfWCI2lG%2FacHaBho8iuQpxGcfAtcsjKUUfmDIDQt0nRRmSodBgUxygVPMkVMhB2InZHngD4nyAPz6oKbFt0MTJ7kKhUzDvYcAqM8ZH&X-Amz-Signature=1c0d8da4dce31d28cdf3ce2da04b1b6b735fe1282c9600b4db7bc4d60440f016&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

