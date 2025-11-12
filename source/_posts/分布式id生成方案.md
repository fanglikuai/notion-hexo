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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SE2B2RTH%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T180048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCIGUh7JoE4eMw5QJom2HX%2FOfZwBmd05tgKacLbBgYG1RHAiEA%2FqoBXT5%2B2C%2BqZfWLkmmKzuBSaZKggDEW3X17sEHcVloq%2FwMIOhAAGgw2Mzc0MjMxODM4MDUiDAsSCFtQsyImHSKCSCrcA0J81Y9LM2Mb01AjmR6JIz0b7fLeRhxy0NGKi9VuytwRBQ%2F1NqBbyKkjVNiLgdsimgciCy4arxrjHfuXuqzSNwJ0sA%2B4LVsy18AFUZLogaUF1Uszz%2FAf3yPgop%2FjNnxjlmfV9nQovTW3dWEx9RI8tB%2F1Lp6Kvr%2BJPMh7KpDmMQgagAK9e6oP8liuI%2B1dxia%2BJKlFF%2BTCWlZQQQSHBxGoqpVfq3IfRVhi2tSF%2BscS%2F1JJRMacSrEQrpZ%2FzJjQDvFCOkdGzD5GzQBMYuSw3fCGGR5ty8lBwH%2BR3GOioRqdEIzfiu%2FVxtWDRwXnZjKB1foDXEMkHMlIbZun72xS0iYyp8GRMVc%2FWQZOnNVDsMau%2BkcgmbbXo3ltcmjrqqhtYIjQc7L9Fq9tZ5Ew4cwvqGxuUF6S8dbQ0E7q9ONSa3kM0oPvlEXY6eW0ABLZTlZC%2FIaNuO9ofUJjOtFn2F3fOr0CTFPha4%2BoInUuw%2FZ2oKJywsWZDNwqNviw3a3BQJkwafUHwIbDkhnFAqa8cfKLH%2FZPCyCAIjTYHZ3e4NXgwuF4HV8Hz%2FKL%2BwgnvOgFP7Zmh8aZnQ34FgCYrOAq7kmLJiZ6Lh4Ab%2BF6B2Oq0qSUg75qM5OabwK4H4k90YMbjeyWMM%2F50sgGOqUBuvMFmP%2F1PPWaoAqOS2BA1ZdzH1Hk2Kqg5viQDw108rSBw2XQFQSRPtsU1ekSbC2O5hNsJKAjIF7Kh7%2F%2BorJmdJsUMrN8o0G6zI2qiGhoCReJQ%2B0Kt2Xxs5xNmsd8QXKHx2Wct9N4w2Rk89hGttb3u2dNDFKWTQo77ZROSr5D%2BPwsy7nqMz9zkeiiZV7HfPuBMK53QxgS7egCEZkiJcPNY5L1fGQ7&X-Amz-Signature=bf9bc0ca39eadbf2dec33468c57131a4a2217dcc0648417691939aa289151375&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

