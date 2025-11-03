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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663BBGHAOV%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T120046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEKvsjjoO91M0WGo1stJeIOdh7jXR9cDB9JYGvwyxHI0AiEAgg2SRnWoZg1%2Br0%2B2rm%2BAax1FQdEhStUxCaPyk%2FhjVj8q%2FwMIXRAAGgw2Mzc0MjMxODM4MDUiDJa7%2Fz3gLSE%2BIXaQsSrcAwQBsMj%2Bn1kFozASqHcefWkPC9bYtHbTvJ7fCv4%2F6t8GKsRyW7qtF5w5GsVOXIaLx9nqrXO7UdvXIG%2FSueOPiCDJSngKu%2F98%2FzI4CM%2F2ZXFV6Hh%2F5WwAzAWKTv462MxrP797L6HWzwz6aMJB0fE%2FsmUsj4gAXuqL4UDP8Mxck4JB%2F99bEIHhiJ3jSI084n0z8ubaQgxHOqDTuT6KyITCX9rmtmBc89qlRV3Gjya02Xc5XsWVOfUWCtwcyohGQ0W7D2OkUDsGawbCO%2BopuoWwzA%2FCqzS34VNb6dhoQYsDb%2B0%2F54K%2Bk7BmBS5A9sriLFz2VQCymfEFs7bjY3CJ14f2gyirsPJWcyyTJv8XHGTDzivikvtTGVf5edAX3E%2BMFKDEAcoLLbNWIjdb7x70w0EUxS9K1eMpuDvIrgOr1bH7WFdMY1zOvXOUFzaz7ih3DjFik6k5pOJa5LtaTrt6B5tOss9WeAz40HLLeUGdFN7CUbHPZUZ4qNDtW7Q%2FkBNVTDTjg4uL9c%2BxO0y%2FdGo9jwb0H2YKsTblqJgy5zefNZ%2FseImmpKUVNj8Je0bH2XKz9EC74peVpWbMoqXwDnp%2BMiqvwEuDWM9myrIemSvhVnFByCCD%2FfkBglPE8bVHYNekMNeiosgGOqUBgxOljVC%2FwFa7jF9K%2BlavnBiXtV%2FqhmtVVJhWmU5a0l4i8OcAxmGK4%2BaDEmQ4CmX3qDQW3JhAESxB37FRFo8cHAFWQXyk4PzU7v0tRD2oQdlC4fTz5RJypzjx3PU4jGr3YhKG0qIqGXsMgPM66ztPOwgalIWXo5klC0YHuJQSotQT6zscokZoxLevWy6dYCieJvZBOIlAjPprT4sOu2LByI7JsiSM&X-Amz-Signature=f5a3923a6d83219acce656db90357a8f36b3088d340883643b96b56664bc80ad&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

