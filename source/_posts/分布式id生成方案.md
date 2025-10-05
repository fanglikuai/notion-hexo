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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TIZ44FW2%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T220037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGIzuTagkvvnO7LUb%2FCYMeY8GGj1iFfB4TILY0PaA8RSAiAwhlBrlgv2gdGxMx2tUZdaQQeUgNCXukU9e1WoG36kuCr%2FAwh7EAAaDDYzNzQyMzE4MzgwNSIMufnFe9wLQVYV5WEHKtwDVTPb4OyjpFXeyYvo632d8dlgoeKmFSTzHjrO9PjNucBGEsvjxOzxN5wcfFc9UvJU0RfHToIG681Qbth0aE5I6%2FVg7DTl3%2F7uiGR8Fr0DkMNDSrSxK00zUp%2FzmAqehsz3%2Fnks5%2BOZfQ2c08vJH%2BZ00jjBB16wxspAN2HOx5tfeVWSG7I4DvnIl6BcUe%2FM4T1XsL5ZKY%2BCcfhDJbogvBuk40MuLmzv68Ypt7HncrY9ASGQo%2B4PjPSLJwrD6EBArJ9fSUUbd%2Bf7Qsyx3kDoYQ36oeTxFO34I3br9aHCtgbw%2BS%2Bk54txTEQavGN4Kp%2F9xc68G0Yj3CpDaAn%2BH0pS4hfR4ZGBUJ6H8YlrJBlLtVEdnEGolTVlwfAHdDukUdqCup%2FBdEWXurHjCn9%2BgB8%2BjAjKwVwVsXHSgU4ULxnH%2Fz2fbXBxRXTIeAcIWujhiyEzb2k%2F5cEoVXUUyQsMU7Q0q0CEoYCiIUEWvOxt%2BgtS8B%2BYwZ6aQxIYQshgwpH0vW%2FmJdbPg0RJZZBkUWTuwuungHs4ozg%2F41fqxYC0EGxZoxhrvqEKcsi%2BVeq%2F09DAXb0U%2FyBcU%2FafKw40heQZPmWYtwrrMq9Q%2Bc%2BadmaNhkqdy08AAx6j8yAsbPYu1KRInjcwseiKxwY6pgHq%2BSp0ncrzpmSExKrrpEDI8w2t%2BjBJvF%2BPfuG2BhgClb%2F%2F6wT74spKXBP1toXhb0BOVEJK0Fj4%2BNf29LH%2FaDhEh9FxNVyhAFUs2JldZpl35Km1fyz2NJk3Pw7LDQ6hCA%2FURwdr8fjzis8CMCTSZdbnlN3j2sdVVtKWgvv74VdgeovfLHc7pqoQUw11UMZ9wc99p%2BYvNvLvyBduq4D40tp5hR5z6lqB&X-Amz-Signature=ba72acf914e571c7c9bddd1c568ed3009fd67ad4640f14d65d3037b33cc83de0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

