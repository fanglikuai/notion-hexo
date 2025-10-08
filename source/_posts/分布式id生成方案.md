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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662IB3C2LB%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T150043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECYaCXVzLXdlc3QtMiJIMEYCIQDyrrUHtt%2FkETswBD%2FF7OntmaAuAUKAumzZeaETTm5ClAIhAKUzn6Wq%2BXNPcytZ47aBLz%2FEWrYoXwh8sfsfTAb%2FrohiKogECL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy0IGdQKfSMbCuRlDcq3AO8t%2BgSa4hheSTBFgenQ39rzGhVbVJJVsCpXHvX3zO95Wdg37c48LrmsdGdax0MGiZmai63fE0Gnwr3l7Ri5tDfff96qZIqTzCFhZ2EEf2XmlvM87q3VqqtqmG%2Bf0ICtbrJ7Q1HqRFZRqznGAQG1ydKv9QmsQ7ZOrYc0iSY6GU8lEG4CY5QG70xwLhpardt0Sc8b9xTBfAK%2F9gvjAPsBQIkDuhJKqrVWwEVC6khusgNaSCTvs6pWxU6crMmBAbJRx6IiAHR%2Bw3xSJUYhE0RucL2n3GqQeSL2PGY%2BkHIDhWbqJzSJHqqgL0BlU%2FbqOYlAc22Ihsva8LyYISANSqtPpzm8oV2eauEEBNx2PoEtPDcRb%2BdGeyBVwCPN2qVPNWAaIqSEXnkm5KCiFT2B8VafmNQwMPw65SrNDg4k6V05Nxw4N7Xidg7RcSSlJif0i9%2FjhfYTui2lhBcM6t5j9x35YY6OGn7IT5Bw5ZUMeNSG1eoLrRt01FBRK4u18PRKbnBIzdSyMTi9jurnBphdLpt5Cj9m%2BvXsdSEYWHvmBSGVnCviyBmtToWs%2BNRsWoXESvl0Xz%2B2SWARbKc7qV0YlSrSfOn97F42IQXM7taTOSk45DL94G553TcSUhfDK4nsTD42pnHBjqkAdnZmbt44vWujK5f7ZcQxFUm6sp%2FV535lTLOopWxzBmdUzZ1Y2bAODfZo59FhXGNOMRdaBprv9cQ9bESoIJS2ottl1fEBh1PFEqKBYe%2FumBZAt0BsuGx8LRfvSv0g7wIF49tRZKIHhQIQQKXIw8aBaejVRG3tANpbc7nwwEJ6nmN1tHSepQkrwTqEO2JNyM8QQrL4Ac6cLiIXEY9k3ROokZ9hXDI&X-Amz-Signature=5cfccede0712aae49add894c2b953ff54ba108ace9a5de8231c0e5d6f1928727&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

