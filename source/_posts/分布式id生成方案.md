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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663YXX4RSS%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T200040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJGMEQCIAa359vvNe3%2By4bOPFtz8Qz7lcuyAaiUeR3GDtXpDr0YAiB87H%2BXWVSA2T6AexZqGths1BuhwbBemq64qHV81eXKOSqIBAjx%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMbWTxzmOEduKgVe9SKtwDR6R1g3GJsjrWfN2X5U%2BWV4QtePVkILBP7O7e3Q8dvFOsachfDDQsAsd%2BM1hbC7yJhdh6DVujK6ar8bAn6mU7Jl%2FAHQ5es5Y15v3kWzY%2BGdx0oFZZYx8itgl6B6ooLdr0V4SpM96m8pDLPtZ3nb1STN6GS2dLisGkU2u0R0UK0%2BXkBy4IyZHve9Ge0ScnuokhrboqGRjBIs%2FV6iQ389mmDxEug5MnK8zOk%2BmAu1RCjooVIRq74rCsUKLJKwbmgutrV0GQsx3WFHnhBMVW9qsatVUumrDisDJ6xk1p%2Bpw6AR74JIIArh%2F%2BqZ%2F3ds5eyPOzLmHHfQfsdds3oF%2F0Fcd5VJGw37wFrGc4DAH%2FQFjD31bq8zrfOvy7k1zFdFX%2BHQXXwyV%2F42OJWblcUesff1ABQ5gAUI9X6Rb%2FYm6FnarUjpOzDl8QQsKhPO1F4q6CEhZwcjyj3wzM%2FxuiDMPbsrGad6DEcrRvkqY38mtQzSjOCKFLnanHZeO3t1eWdEtV6Mlrp8itrtKvr70uH59MA02R99smI5lWrbdPyJe8gvYluFGBl%2Bjx8UdAIH1APmLCMn3o24zv9m2aNM%2Bozfi34buer2c07d48q0swJ8McMW13ta0g00tvXUe82fdrOdswoZyOyAY6pgH2rvu9M0CfHJacNTZAiHF1S0lLENhZxeY7fz%2BuQORltY8cnQ9Z6n8zuTM%2BNpmzL8qHyRvynUsxNqkm%2BirTE5q%2Fkf3yP8vWi5CSNenZYNs37bfd3FFsU9gOJ8YYh8MfiJ3AHjJloF1PFY1Kh2ZZr69Vuf92XjfBvyM9GftXMSkRwTCPvEvG5xOhdg2qeR5BOxI4mWVp1sJ7y8EuAD7JFTkD7y18%2Btmq&X-Amz-Signature=68a6daa5a5ccf252388b0fc9b2c03504fd716b4589d94476c7aafe69fba7d8d2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

