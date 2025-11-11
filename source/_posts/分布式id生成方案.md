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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQQSCY6I%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T060055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE0aCXVzLXdlc3QtMiJHMEUCIQDzo6LEJpWfBvtnc2V%2Bw5NtC2qyeE%2FIc%2Fs3PXZ7UAKjUgIgLJ6S65dAFCvZIx3U6bVPla0%2FM%2B9XkfriIwMpQmJbZ8Aq%2FwMIFhAAGgw2Mzc0MjMxODM4MDUiDErUmEyAXJDzYnuGryrcAxoSsnv9yZACjcFtKbBM1jPzzdQ33NSU2U%2BBzJYIMlem7zM2GhSR3BGgDW81aYz4evzsfCPDOnRq1L72YEjmP7w6J0e98kGGVJMRR%2FfMYi0UoMn0QtI2Fabj3UL4vF6%2B6AgiWI%2FKDzLrqb2XR91YmqGRAApU9TlgrHa8LUWgDPwjHhXa23Yeku3km3UAJzlHEHIDDlw8zsBjs7KnriQTFv8Ix73f6D7kW1AAlCPcvXQYNnn7LCYb0uW4GaBYxq71Cq1IYHqI15Etd5AyyHwEZ0U%2Fou4qlyuuowxmY%2Fz31XvK%2FbzuH8%2F5M%2BmAZzVgTg1s%2FJmRJ%2BP9Nx2%2BM7BKqOZftBl0aOL2y2AXqVeRWIzhhlUD58ncy0uzNKL44a52LMZgNIUoLTgNbfVD5SKHkB6Fg1dvdtj5eq7NhXQ8SW7pjH9UKbsmO8thP1r1fqme%2B271cTSKL2eOd9MUZv5B5MhZ1eZdtfQCeMQIFKrfvmldg9dIWPNXgIee1JiA%2B2n5ilp09Yr20HgbR3Z3eNICNqC9DjdWIww%2FWo6hkOp68qlizJDwfyJPLRWJfelXk2mP9VKfhAEcLMWjPh50ECPl2rOD2INMUsY2W76bX3KRu4UAVL2ceQIQ7v%2FAlnE%2FIDv6MLCEy8gGOqUBaxK8AUpkmzPN0MMlq8gCrvVOThVx%2BCLLKatJtK2jgMqaWjEf6Zg1Wx%2FWqA4tpBsD5C1fNk8Ft%2Bf%2BxhmoBNI%2FJXml4mDv9%2Fmc0SwjnUrFjhImg%2BupWr2oZrbsoeXwGbDuNjnUZOHis8chPH8UuYcjDZ33aC997OAyp3xFourzEDZGJxmg74mpGMzfl%2BM3omZ80uqMTRImNZ05Mg8jSVxGJeDz1Rl9&X-Amz-Signature=b4b6a858f3e32911faccdc964c9007275240e69e2698e88bcb2393abc765ec66&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

