---
categories: 整理输出
tags:
  - 分布式
  - id
sticky: ''
description: ''
permalink: ''
title: 分布式id生成方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V6YKDKKB%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T100040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAIaCXVzLXdlc3QtMiJHMEUCIDBKOYPQ2gdPRurI7jBARQ5oR%2F2yFc7b7mezmMFNQ5W3AiEAwtUb0n5AWhoQlhL74Dobee6hplaOhyXXNn0ucbenpvgqiAQIi%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBThj0sZzanCid2FRSrcA58gcu8%2BIg7hcPEQf8o4Dc5kDqumhx01jC2Fg5DGznZLieZOPgclcecqOiJjjhl8H05Y2dwUBlvMzIbPmOj%2Fx12Ns950i%2Fz4zqLW5jYTK90qZSq0hzEBTQ3EDJbUEwBbFRZ2VK9GzPN4MVEXsYtECQqjYgWuBrlDT7gMrg9l96c%2FZT8GDtiZjKinYJxeWJOgXuW7vu4HdH5WrDaX3dFrN9Lb%2BMtTWv%2BMlvY35qePkM0rAmPQv9wiyvZSUTIeZqD%2Bz2TL7Y7ZMZ7zByPpcpMQBxFi75vYKcbMPOA00bmDHMYS0EKzJb1B1DNg%2FQWTJXn9qQlHMnZV1mof9i182qz%2FdbWBMNJSAIhwav2wDkxDsXWcbCCoASqQIAYDRmDKr6sPQJo6wQlIrckGa93FyZYt9jVfHiiK8elxnh5DeKj9zC44oSr6vFy9bWUPl5r0yi2AK8f6ofvJKtheE5ltgEYMIXALjflCOZO49IZCFm2zOQsXHrSBHmWle1OIU15yWwhW46kIKUg8SUJAW28I1%2F2n4%2BilBJXAdOo4nmuLonfyGD7BRwnw8L4ZY%2F5kdbjN1Qd3VFVExHWqJxCQPNl6ROT2rLSd1W%2BZ2pNCJhXCc2lL7YRVbJBkup%2Br9e9kq9jgMPi%2F2cYGOqUB0Bz4CEXqHJN%2BiReh%2FmEAi%2BMno8juvRW0UK49sMRpXFK7d4cO3XF2oIg10YO8ctlHw3i3HA%2BC4wcYX3IvEqyzRSHSC4NBdK6%2BqLFZejLowOzsW5YPNB4NywfZ5npS0YvrXId9wwaqBYBchfkKUHUINsOsEufg6ruTgIMFPwaWBKXsQZT%2FSeYfbRISWHi4vNcyK3o9oERqsnlKPaL%2FIr1l3hFt%2F%2FAm&X-Amz-Signature=5c0f4d961704ff111fd995d7701ed6c19c8d988968f68b24fe5b7247962a7594&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:55:00'
index_img: /images/62c2d2c32218b2c2c1ec4c8cfc657330.jpg
banner_img: /images/62c2d2c32218b2c2c1ec4c8cfc657330.jpg
---

要求：

1. 全局唯一性
2. 趋势递增
3. 单调递增
4. 信息安全

# 雪花算法

> 0 - 0000000000 0000000000 0000000000 0000000000 0 - 0000000000 - 000000000000
>
> 符号位 时间戳 机器码 序列号
>
>

12位存储序列号，当同一毫秒有多个请求访问到了同一台机器后，此时序列号就派上了用场，为这些请求进行第三次创建，最多每毫秒每台机器产生2的12次方也就是4096个id，满足了大部分场景的需求。


## seata版本


将时间戳和序列号连在一起，然后每个微服务开始只获取一次时间，后面直接递增+1就行了


并发量大大提升。


问题：


如果超前消费很多，然后系统又重启了，那么是不是就重复了。


不会出现这个问题：因为这样的并发量得很大才行


![imagesb91ceb2795145be127635d50c861bbfa.png](/images/c686239b1567bd4f1ee7f6da809063a0.png)


最终收敛，防止页分裂


# leaf方案


## 数据库方案


去数据库修改字段，每次获取setp缓存在服务中，减少并发量


重要字段说明：biz_tag用来区分业务，max_id表示该biz_tag目前所被分配的ID号段的最大值，step表示每次分配的号段长度。原来获取ID每次都需要写数据库，现在只需要把step设置得足够大，比如1000。那么只有当1000个号被消耗完了之后才会去重新读写一次数据库。读写数据库的频率从1减小到了1/step，大致架构如下图所示：


![imagesa73fdbbf512d967222aafea71d8485df.png](/images/6ba8f71fc9902de8f6ead17de802a727.png)


### 优化


就是监控使用的id量，到达阈值的时候，发起线程去更新


采用双buffer的方式，Leaf服务内部有两个号段缓存区segment。当前号段已下发10%时，如果下一个号段未更新，则另启一个更新线程去更新下一个号段。当前号段全部下发完后，如果下个号段准备好了则切换到下个号段为当前segment接着下发，循环往复。

- 每个biz-tag都有消费速度监控，通常推荐segment长度设置为服务高峰期发号QPS的600倍（10分钟），这样即使DB宕机，Leaf仍能持续发号10-20分钟不受影响。
- 每次请求来临时都会判断下个号段的状态，从而更新此号段，所以偶尔的网络抖动不会影响下个号段的更新。

## 雪花方案


使用zookeeper，在第一次的时候进行注册，然后后续缓存起来机器id。


后面进行周期性检查：看时钟是否回拨

