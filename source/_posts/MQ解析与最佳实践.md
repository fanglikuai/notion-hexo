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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VV23IORB%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T010044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDTicTPt24XeEVkVoSl2YAg0vNMJBMiwv20duJwc3TZCQIhAMqJUwO%2BvhuKyQoVzxrHqk8U1fO7a%2Fa%2BTYQOD9rRcyaBKv8DCCIQABoMNjM3NDIzMTgzODA1IgznKrEk6oQFMLXTojgq3AO2QkAAaSB1PR20wWCnlmK33ic3nMW2toI%2FVvWdLn76hzaJDfenj0eDkUq626c%2BYkzNSDX3vcEUCvYdfR%2FviAjLmK9UDQS3VCtuQ6td8ua0cIl083M7CPuQsCPPkSpnmZxs%2BqL7l3UyNGFnWA2KeueNawnTUfgKUzXYabCWdZp1llX3jOUHykTEsteiJ59SPPSdfdwu7ynvzgfy0keDHNaTuDaLB1MknNknd4eFftiSApH0O3pOK7RpYxyG4WF%2BrE%2Fz2ECu6zoxWP3V0G4ikKKBduhxNj%2FeuUTPsnrFG2bCUyii9sA2YUJVZ96aYo3cRgyEZ%2BM%2BXDASdmCDyoaJqhev18VBggllrUzAzyTXMY%2BoiwRa%2Fdk0GtyBAUFxYpVhZwLUlWmPs6FChqTyZZCwl3LUNXSuXIYDnCc2sr4pdXXqSN2N%2BBjO5tJ7Pz9kC1nM8e%2BJXXJVteKA6p2QY0odvNG%2BwOmwcDuHgTL%2BAzcdgI6bEJiF8kvPmupyP96wOywv%2BexniLwPKUXlFcFOZwZS0bqmVF1nTT1f7Jlca7NRdkIohCwNtSwjuQN1uDCcBO8GJZTDshbCbhCzX5%2B%2FaQxZa%2FYJTRNzI%2F85TkybhRJIeMAEOMmv1WiFXa3q01tmWzD8m%2FfGBjqkAVrXI%2FPizW%2FfpFVY4Tp1QdgvQmMJoxzW%2BV5%2Bxyci%2FTDpAdBR%2B25O%2FdesMVAZuyTzwleChegnhCtLJ81sD92sdsUEx%2Bom8thWIjKuzCRMMfFmmh8R3r0mwcLwVa1F3g5NKl227iNOnHoLgGMnwYDCbbZ%2FuReiw8JtG9hb8ENynL7%2F8u329t7xfIK35y3hKGANWAEnDqF0BZrOXKfIO9PMnNRhpCbm&X-Amz-Signature=3cf3b707065ebb39511a64cc151d2b1ec97361981d8244e435dae148995f25be&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

