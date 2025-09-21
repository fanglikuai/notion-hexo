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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662DWQSDDC%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T150040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCqaNWzcQtoANzNzd3vizcJ%2B4URyXkybkYKQ1EFiJg1cwIgMMsIgk0i5M1%2BTu1fFI4NHmkr2c4MQaM4qa5YDCdEUT4q%2FwMIFBAAGgw2Mzc0MjMxODM4MDUiDHhuLC%2FcjUko1oY8mSrcA2p88b7JO87dJQjdKs3i0dbXGuX8HMsxmmzXM47bJUKu0OJgn2QnkO3qXtyYuhefs32ZCOaoT4s8avi95PJEhhhbvncfND2SalLeimjFUuJPbDfAQCrMdI8KWVzUv7T2Eh444UC%2BRHvo%2BKg8rasLlX9n%2BEe36j%2BqPPlk%2FEiHCiqRBSVLMz5njy2MqvVjEGLvQrtwseSXyvdrNzGcJQP3xNv10fsXN%2FrD1nKZ1mjxspEVAwnmDu1%2BE74eiFvopH6mC0qeOcWy6lol8QZIxw6nO4LHsxe6oRRtfffIEpManhFIyNgQ6E%2BBOhC3Tn45vAE5QmymBRhiO%2FACeaJOcCy0ujwvukjK4Zq9ZV7blsO5M9uzKkzcXO5pKYKcJ38XEmuXZNZ6LRWuM%2BLvJ7teYidYeuI%2FP4OJOymJ4bRtx0g%2BBYUn4Gn2qTpuLMYp0TpSPXPl5g%2BywFMd%2FfhhCfijd1F9Gbi0N31od3%2FumAVrcQUOvJy4OFR2E%2FwVumP%2BY320l21cu3kQSBuYwNOlP0g0GmZHuqVr78A5P78GUUScaWcSk4j%2F9mt1uZqNoc%2BngN59mo%2FInX8feNPtAZVVOOQLbD2Ueh2MkG90ALIpP4f0dIg2F6gllMxkNI4MrMj22cVWMK2kv8YGOqUByA5J14bA6jHqjRCAUQa7oscgYHiMP%2FQmRa%2FzZe4PP7NAxumOJqSMaEM9GScFL0Fj2ffKWFyqMmU%2BWstNQVaC0YXfxIx5htfGN6zkiXvNrfNIXBcSsnx7ilpCy8wakSdONx8%2FwfQFup8jrXz3zcmNQ6E7Cx1uL7wOOl2jvFNjf%2FYbGS01hgaowJnbCIUPgJQ36Wda3%2BqRzAXKYpjjWtj%2BURKa1hup&X-Amz-Signature=84860bbf8df6589b9ed92a702a6973e49130ea6e5143bc749a7f871c86b04ddc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

