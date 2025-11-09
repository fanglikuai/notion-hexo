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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XNHK4AU6%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T110039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECAaCXVzLXdlc3QtMiJGMEQCIE5gHKmLl9j2bdIcfFSiRfukUqkpK7wo8jotqO9SyvjUAiBKzNs%2FJxOlfVbyEDvJ7CX%2FKger3a60mlvf%2B4130qPPpSqIBAjp%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMrkJOu%2B%2FQWkIL10eOKtwDzdP9v092ErMDXPNTPlj3qXlThiooP6vN3811MLREbBPWD9Fz357Y4%2FjDX4h%2Bde2DIFNpqcqkaiVLHdXbD4Fu0X%2BnZ3NtOsFrFfOoU6sBd3r5%2FxZgBeatdPjufUiKCKYJCTgi69SK2NUrRcKopzPnyhNB8bCiLhqkGqHxDgHKpEnEb1A7topS0FeaNQ%2Filk9hSBjunR2I4wBvtqFifGlpMmEiz12EFxu5U99JqqddRU7niiFw08C0OdBF3SL3LeutWxbxmb653bfpeWTnurV1l70s50r%2BENB8Rj4zFu8flOIa0UobN2dRli3Eb%2Ftky7f0q19Sx6P39FAuza580WZqGwl%2FKcS%2F%2BxQrg%2BTkpjzgkpjnaGuQpO%2Bha6gQIXeL2XLjl26GWosU7HFny5Z8d4B1Hh0grYkvi22bLde4Uauc9OYcuBV883Qi1mdBlPxdwg6KE085mIUIwImychxStOcq8lHSc4GceNWYfK9UiDXRrxfShC%2BgGwu%2FjNGzaLkVn1PF8LSyal4ub52U6PsLnKL964uNpL4raUWhQnFNhSJOFutmlN2tWrHJ6rqu6Vpwdlil78Ipw31w9qWinx5fzKCLgpw6%2F7NMF4apQMLFowqUajSGP7bwm7O4k0DTWjMwmozByAY6pgHgbkeutQmsR%2BmUEIZfS%2FkB6rFWEnH37Bhdj8J%2BL8TWHfo8KozfacnzuHcuCw2zFOtRZlx4N466dxIQO2IT9DXFO6Aw4rZmo%2Fircw1ivYiOVD7pU9FvKMxNJxBclBrr%2BwJ%2F1Av7NdWO6WGmYcrO1yLmFLK5SckoyXTBg9V9Qu33dNfzonDmYngBj6MJN27SUeZfREiMl1eXBSDvTxrOXBWZNGCC1i7%2F&X-Amz-Signature=8501f633051516d578bb520f545ff35040876c1cfde68e9195739eef52297dfc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

