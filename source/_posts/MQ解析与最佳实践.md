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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664OHAF2LV%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T030040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEst0%2BtQM%2BS3fT8zJu26zsHb90EhZdnZXVpTGuMoUgSaAiBjret5OqdaASgqC1siMhh8uDxbJCEhSBiK9Ia8wm0sayr%2FAwh0EAAaDDYzNzQyMzE4MzgwNSIMU4w3DjOsj5wQEMHyKtwDSDAOkQLircbIS38fG8TdHJEXMuZp1d4Y%2FIJTAqTZogjs5bv3Jyji5jJxg0NHr%2BZkWGMHQvJbneCE6wufqBHlxgiL%2F8EV5J3qL8PI4jn21qjrxhv86RkSLZYUXiRocWPDAON%2BPOqKkna7IkGW%2BLLuxl7qJceyrBOvMiR25JMibWhTOEeYpR39lEJsBTR5v7P%2BvFY1dRU1dCD%2F%2Bnt7sseb7mqzfyZlOxVP0zhFxYBJxeqvFi4dGFeDKoeMiv8UegTpa3druo6BDAtRrOawagsoq3NCZ5FBQJ72T48GLmHJ4dMIIoemyg2nnlR6%2BpOJGRK0%2Fic19H4%2BzfrjM2wNsVrddRzOYLsxtUdYdPPfxEO7agX%2Frv5qJHh%2BzdhWeoX4%2FcmbfwKB2tzE4WOE5R45YbA4WiplaLLGv44%2FARUwhVcaludieINXYD%2BhtHJhwA4LsMDjrBgamWc7iqdX0CCnqsJoXlUnhJTgHn7xQG34BfeXEMHFcXmrOfFFciGa00lIwc97QkeoF9QHN5dq0XXEQ4702m8fNjPdnr%2B5rxktbpVeA6ErRdGLk789Qsumj%2FeXAzaZBIjs5AjylZyeBQv4p2or7hgElvoaVpcH9O1c9pgXICSsuKQohsxRl9eZiuEwnszfyAY6pgEVfgGN6mbrK473ZUQZ7BL6%2Bnc9TAbX%2BDzw3jWkIuzK36w3vpp9A6LEKFHwjZnfMkxvbeL%2FllKELccPTt8vCS5PFK8z0HEOa%2F3TbQ7UJiaU3rajOIaYuvoDZQ5X1QhEw%2BtACX3vWp0ZDS2%2FDz0eOYpYvb4LmaY47Ko8MkTLNBF%2B7i8%2BZOthRUefXZoccYn4fftsH2m%2BV67%2Fl0GbeRAzLfZjRhdcliTJ&X-Amz-Signature=7732b829d56b49a38bb0e2bdaaef55ae70e601b98aa2499b04519ebadc5c8e18&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

