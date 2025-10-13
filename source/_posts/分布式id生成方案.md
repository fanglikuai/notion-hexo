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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46637UPZEIP%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T080046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDWLP6O3KOc%2FNlN0zLtZsaOUEODTyN6lnmU8m43n%2BWRfwIhAJ2kSq%2B1r87FD5bW6cGPik7r719iiN2KHhuaJkbtUhbWKv8DCEEQABoMNjM3NDIzMTgzODA1IgypUP9f6tnTBnvuFHQq3ANFMtck5jrTqnqHeprDwDUeTMReJJv2PQFxSdgKLHuZ7i5I8fwzBLzBp3viIpcOxCQ6GzY3KJ%2FyLAbjqmkHeAVhcQtor0Thk60shYvV7Q7OJkgF%2Bo465kz5IALzvqFwXvm8nKU9kBwCW%2FWiMbBUPepDq80gHx71nLasmHIq3tRG%2BbI0TfeuyCOb1GjM%2BeTH0NwuFpb%2F7t0zp61RjHrR9R%2F3BPV%2B2YPxfX6S9%2BXSokXinpJDe0nlRHtiBU4L9UFqAiT%2B7BlQbjGlmTke9wQvxDW5TZw1qnSzT9D158BSGOYTrUxmGrwpV99FVhOVp%2B%2F%2BvkTQSNnR4%2FBA2bZhYxzqTUfbuWTMxbR98s48PT6nLLPw8EHfu9DzjUTSuWqLz1htWZUaalRQredArsoWLtVlE8DQJ3%2F28hu3ajWDc4Qyg%2FBzvdhKcz6EhOICcO3bDN5RyOP3UjBuOsNLiqMF%2F6zyfMq5nOCbUqryz5AgRwUHnW0K1PMWqmuhbIgdZ25zLaSMnhzcue8d7N7KmB6XPSrLNXLGV%2Bu7Pgn4EG6x0RtmdBgd6m%2FwJNu6weN2mooCZB%2FowWOiNVIePSVEOy%2BQm5TB64jq2UvcTWwLwSZz8DsGCjiV88wq99VTPsegby2sTTCT4bLHBjqkAdW52geXZmLwwKX27L3mQlT8r%2B4SFmNzgTQzHcNFBbZF%2BECzTiBlwoSh0Kty%2Bny0AcyzeaKeRDMIzHLcMsndgk0tW1UiNp7jBfiQaspVQsWHvFqE6D5zuiYU6n3QnRAL3f0ix25IZmKO7vITLDxPQpaPc%2FignOlCEPM3IocyoKjIY6CK2go3ZdGnpko98RQAYPeDoYLOk43d%2BAATa80AzI0aIfQd&X-Amz-Signature=466b2962a2865fbfc1521aba55ded62df0db2eaa2f0bd5ffbfd2de2e01d44dc5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

